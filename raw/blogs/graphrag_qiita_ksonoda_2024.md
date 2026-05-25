---
title: "GraphRAGをわかりやすく解説"
source: "https://qiita.com/ksonoda/items/98a6607f31d0bbb237ef"
author:
  - "[[ksonoda]]"
published: 2024-08-30
created: 2026-05-26
description: "本記事は日本オラクルが運営する下記Meetupで発表予定の内容になります。発表までに今後、内容は予告なく変更される可能性があることをあらかじめご了承ください。 ※セミナー実施済の動画は下記です。 そもそもRAGとは 以前の記事：【ChatGPT】とベクトルデータベ..."
tags:
  - "clippings"
---
本記事は日本オラクルが運営する下記Meetupで発表予定の内容になります。発表までに今後、内容は予告なく変更される可能性があることをあらかじめご了承ください。

<iframe src="https://qiita.com/embed-contents/link-card#qiita-embed-content__1bf32c74154e8a509a2bb752b49a07c3" frameborder="0" height="112"></iframe>

※セミナー実施済の動画は下記です。

![](https://www.youtube.com/watch?v=NuuZtadZ63k)

## そもそもRAGとは

[以前の記事：【ChatGPT】とベクトルデータベースによる企業内データの活用(いわゆるRAG構成)](https://qiita.com/ksonoda/items/ba6d7b913fc744db3d79) ではベクトルデータベースを利用したRAGの実装をご紹介しました。LLMが学習していないデータ(社内ドキュメントなど)をベクトルデータベースにロードし、LLMがそのデータを「参照」しながらユーザーのプロンプトに回答するシステムで、処理フローとして下図のようになります。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/109260/6e826ad5-885c-589a-0086-fcd57c002305.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F109260%2F6e826ad5-885c-589a-0086-fcd57c002305.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=90fc8f750d8baecda75a097f37b76ec5)

①ユーザープロンプトの文章と類似の文章をベクトルデータベースに問い合わせる  
②ベクトルデータベースの中からテキスト生成に必要なヒントとなる文章(RAGではコンテキスト(context)と呼ぶ)をベクトル類似検索処理で検索する  
③ユーザープロンプトに加えて、検索したテキストをコンテキストとしてLLMに入力する  
④ユーザープロンプトだけでなくコンテキストと合わせてLLMがテキストを生成する

上記のように、LLMでは答えられない内容をベクトルデータベースとの合わせ技で答えるようにするというシステムです。

## RAGの課題：ベクトル類似検索の不確実性

このように、RAGの実装では上述した「①ユーザープロンプトの文章と類似の文章をベクトルデータベースに問い合わせる」の処理結果によって得られたコンテキストの情報が非常に重要になるといことがわかると思います。この処理を「ベクトル類似検索」(以降、ベクトル検索と呼びます。)と呼んでいます。

つまり、このベクトル検索で得られたコンテキストにより、LLMが学習していないデータについてもテキスト生成ができるようになるのですが、この類似検索の処理は少々やっかいなもので、目的のコンテキスト(チャンクテキスト)がうまく検索に引っかからない場合があるんです。残念ながら、これは「テキストをベクトルに変換して計算できるようにする」という現在のベクトル検索の技術的な限界と言っていいと思います。このようにベクトル検索では目的のコンテキストがうまく検索できず、質問と関連性の低いテキストが検索されることにより、結果として最終的に生成されるテキストにハルシネーションが入ってしまうケースがよく発生してしまいます。

これに対し、下図のようにベクトル検索だけでなく、全文検索も実行して更に必要なコンテキストを取りこぼさないようにする手法も広く使われています。ベクトル検索は、チャンクテキストがベクトル化されたベクトルの値しか計算対象にしません。それだと有効なコンテキストを見逃している可能性があるため、ベクトル値だけでなく、テキストデータの方も入力プロンプトのキーワードで検索して、ここからもコンテキストを得ましょうという考え方です。(いわゆるベクトル検索と全文検索のハイブリッド検索です。)この手法は効果が大きく、現在RAGを構成する場合はほぼデフォルトで実装したほうがよいということになります。

ただ、これらのような仕組みを実装しても応答品質が目的のレベルに達しない場合は多く、現在RAGを実データで構築する際に誰しもが必ずぶつかると言っていいほど最もメジャーな壁となっています。そこで、新たな手法として、構造化データの検索を更に追加するという考え方があります。

## RAGの課題へのアプローチ：構造化データ検索との併用

上述したベクトル検索と全文検索はチャンクテキスト(とそのベクトル値)を計算対象とした、いわゆる非構造化データの検索です。非構造化データだけの検索では上述した通り限界がありますので、ここに構造化データの検索を併用しましょうというアプローチで応答品質の改善が模索されています。一般的に、非構造化データのチャンクテキストは元のドキュメントから作成します。同じように、ここから構造化データ(例えば上図のように表構造のデータ)を作成し、これに対してSQL、NoSQL、RESTなどの手法で検索し、ここからもコンテキストを得ようという仕組みです。この構造化データはドキュメントから文章を構成するキーワードを抜き出す形で作することが多いです。

この構造化データは、多くの場合、JSONやXMLなどで構造化(準構造化)データとして定義され、これを検索するコードを追加することにより、非構造化データと構造化データの両面からプロンプトに対する有効なコンテキストを検索します。しかし、これらのフォーマットは複雑なデータモデルを表現することができないため、RAGの最終的なテキスト生成に効果的なコンテキストを検索する仕組みとしてはいささか力不足です。

そこで有望視された技術がグラフデータベースです。グラフは単純な表構造では表現できない複雑なデータモデルを定義し検索することができます。ベクトルデータベースでは限界のあったメタデータの定義と検索の部分を、グラフのデータモデルに置き換えることによって、より精度の高いテキスト生成に必要となるコンテキストを、可能な限り取りこぼしなく検索できる仕組みをそなえた実装がGraphRAGです。アーキテクチャ自体はRAGのベクトルデータベースをグラフデータベースに置き換えるという非常に単純なものです。現代の最新のグラフデータベースではグラフ検索はもちろん、ベクトルデータベースで処理していたようなベクトル検索、全文検索も可能となり、グラフデータベース一つで一台三役の検索の仕組みを実装することができます。

上図ではグラフ検索、全文検索、ベクトル検索という三種類の検索結果を全てLLMに入力していますが、LLMに入力する前に、この検索結果をRRF(Reciprocal Rank Fusion)やRSF(Reciprocal Score Fusion)と呼ばれるアルゴリズムを使って再スコアリングし、再ランキングした結果をLLMに入力する処理を入れる場合があります。

この処理により、グラフ検索、全文検索、ベクトル検索という全く異なった尺度による検索結果が単一の尺度で比較され、なるべく有効なコンテキストだけを抽出することが期待できます。

※RRF、RSFともに完璧なアルゴリズムではありませんから、実装してみて品質がよくなるようであれば採用するというスタンスでいいかと思います。

ここまでがGraphRAGの大まかなアーキテクチャのお話でしたが、その中の一番肝心な部分、つまり「ベクトルデータベースでは表現できない複雑なデータモデル」とはどのようなものなのか？を見てゆきましょう。

## グラフで表現できるデータモデル

グラフとは主にノードとエッジから構成されるデータモデルです。  
ノードは具体的なもの（例えば、人、場所、製品、事象など)から抽象的な概念（例えば、アイデア、カテゴリ、トピックなど）を定義します。エッジにはノード間の関係性を定義します。

わかりやすい例としてSNSのデータモデルが挙げられます。ノードにアカウントユーザーを定義し、エッジに人とユーザー同士の関係性ということで、フォローしている、されているという情報を定義するという具合です。このグラフ技術は、RDBでは表現できない複雑なデータのスキーマ構造を定義しなければいけない技術領域で古くからグラフデータベースとして利用されてきました。MetaやLinkedInなどのソーシャルメディアプラットフォームでは、上述したようなユーザー間の友人関係やフォロワー関係のモデル化に利用されていますし、AmazonやNetflixのような企業では、ユーザーの購入履歴や視聴履歴に基づいて、関連商品やコンテンツを推薦するシステムで利用されているという事例は非常に有名です。

そして、実際に、グラフデータモデルで検索を実行する場合は、下図のようになります。上図左がSNSユーザーのグラフ構成です。登録されているユーザーがノードで、ユーザー間の関係性(友達、家族、同僚など)がエッジ(relationship)です。このグラフから友達関係のユーザーのみを検索するようなクエリを実行した場合、上図右のように友達関係にあるユーザーの関連性のみを検索することができます。非常に簡単な例ですが、このデータモデルが複雑になるケースではRDBのような通常のデータベースではデータモデルを定義しきれず検索もできません。

上述したグラフデータベースの利用例は比較的イメージしやすい適用領域ですが、近年では「自然災害における地形、気候などの様々な環境要因との因果関係をモデル化」、「遺伝子、タンパク質、化合物などの複雑な生物学的データの相互作用をモデル化」など、エンティティの関係性が非常に複雑になる領域にも応用されています。

例えば、下図は某国で行われている電気自動車の利用実態を調査した結果のグラフになり、様々なノードとエッジが定義されていることがわかります。グラフとはこのように、世の中にあるあらゆるモノや事象などの様々な関係性を表現することができるため、「文章の内容」という複雑になりがちなデータをモデル化する手法としてはベクトルデータベースよりも適していると言えます。少なくとも、表形式やXML、JSONなどのフォーマットではこのような複雑なデータモデルは定義できません。

## GraphRAGのしくみ

そして、このグラフデータモデルを上述したサンプルのテキストに適用し、検索する際のイメージが下図のようになります。GraphRAGではこの検索の構造をそのままRAGの検索に適用します。ベクトルデータベースではベクトル検索(もしくはベクトル検索とキーワード検索を併用)を実行し、有効なコンテキストを取り出していましたが、ここに更に「グラフ検索」を追加することで、以下のようなメリットが生まれます。

- 「文章の文脈」という複雑なデータを構造化データとしてモデル化できる
- グラフ検索では文脈に合わせてノードを辿る検索が可能なためRAGでの文書検索との相性が良い
- ベクトル検索ではチャンクテキストに跨る文脈の検索は精度が低くなるがグラフ検索ではそれをカバーできる可能性が高い

このようなしくみにより、応答テキストの精度向上を狙った実装がGraphRAGです。これまで同様、LangChainを使った場合、複雑な処理の殆どがLangChainの有用な関数によりバックグラウンドで実行されるため処理フローは下記のように極めてシンプルになります。

①入力文章からハイブリッド検索(ベクトル検索、全文検索、グラフ検索の3種類の検索)を実行(RRF、RSFで再ランキングする場合もあり)  
②検索結果と質問文章をLLMに入力  
③応答テキストを生成

そして、この3つの検索手法を使うモチベーションはこれまで説明してきたように

- LLMに入力するコンテキストをベクトルデータベースから検索したいがベクトル検索(非構造化データの検索)だけでは有効なコンテキストを取りこぼすケースがある
- なのでベクトル検索だけでなくメタデータ(構造化データの検索)や全文検索をベクトルデータベースと併用することで有効なコンテキストを取りこぼしを可能な限り排除したい
- だが、ベクトルデータベースのメタデータ定義は「文章の文脈」という複雑な構造を表現するデータモデルには適していない
- なのでベクトルデータベースをグラフデータベースに置き換え、グラフデータモデルで「文章の意味」を構造化データとして表現し、検索できるようにする

という背景があることを理解しておくと、今後、類似の実装が新たにトレンドとなってもすぐにキャッチアップできると思います。

## ユースケース

GraphRAGは通常のRAG同様、適用範囲は極めて広いですが、対象のテキストデータの内容がグラフデータベースと相性がいい場合(ノードやエッジなどの構造にしやすい内容の場合)は効果が高くなります。下記のユースケースで扱われるようなデータの内容は一般的には非常に複雑で、通常のデータベースのスキーマでは表現しきれないデータモデルになる傾向があります。そのような複雑な内容のデータモデルを定義するためにグラフデータベースが存在しますので、このようなデータを扱う業務ドメインでGraphRAGを利用しない手はありません。また単純に、ベクトルデータベースのRAGで精度がでない場合もGraphRAGを試してみる価値は大いにあると思います。

| ユースケース | 説明 |
| --- | --- |
| ペルソナベースのレコメンデーション | ユーザープロファイルを詳細に保存した知識グラフと統合することで、GraphRAGは個別化されたコンテンツ、商品、またはサービスの推薦を生成することができます。 |
| ソーシャルネットワーク分析 | 人物間の関係や影響力の分析、友人の推薦、コミュニティの発見などに利用されます。 |
| 医療診断と治療提案 | ヘルスケア分野では、GraphRAGは症状、疾病、治療法の関係を含む医療知識グラフを統合することで、診断のサポートや治療提案を提供するのに役立ちます。 |
| 法律文書の分析 | 法律専門家にとって、GraphRAGは大量の法律文書を分析する際に、関連する判例、法令、および法的先例を知識グラフからリトリーブ（検索）して提示するのに役立ちます。 |
| 科学研究分野のデータ分析 | 気象データの解析、気候モデルのシミュレーション、異常気象のパターン検出、生物内の代謝ネットワーク、タンパク質相互作用ネットワーク、食物連鎖の解析など、生命体内外の関係をモデル化など、複雑な関係性を持つデータ扱う科学分野のデータ分析など。 |

## コード概説

仕組みの章にある図をそのまま実装してみます。  
利用する製品は下記の通り。

- グラフデータベース: Neo4j([https://neo4j.com/](https://neo4j.com/))
- LLM: OpenAI
- フレームワーク: LangChain
- Python環境: Oracle Cloud
- データセット: wikipediaの [サザエさんのページ](https://ja.wikipedia.org/wiki/%E3%82%B5%E3%82%B6%E3%82%A8%E3%81%95%E3%82%93) のテキストファイルコーディングの大まかな手順は以下の通り。

1. ウィキペディアからサザエさんのページをテキストファイルとして保存
2. サザエさん.txtをチャンキングし、チャンクテキストを作成
3. チャンクテキストを埋め込みモデルでベクトル化
4. チャンクテキストからグラフデータ(ノードとエッジ)を抽出
5. 全文検索の仕組みを定義
6. グラフ検索の仕組みを定義
7. ベクトル検索の仕組みを定義

これで出来上がりです。

LangChainがサポートしているグラフデータベースは以下のマニュアルにリストがあります。

[https://python.langchain.com/docs/integrations/graphs/](https://python.langchain.com/docs/integrations/graphs/)

#### 前準備

必要なライブラリをインストールします。

```python
!pip install --upgrade --quiet  langchain langchain-community langchain-openai langchain-experimental neo4j wikipedia tiktoken yfiles_jupyter_graphs
```

以降の処理に必要な様々なクラスをimportします。  
全てのクラスの説明はしませんが、Neo4j関連ですとLangChainからNeo4jを操作するために必要なNeo4jGraph、そしてテキストデータからグラフ構造を抽出するためのLLMGraphTransformerがキーとなるクラスです。ここでLLMGraphTransformerはlangchain\_experimentalのパッケージにあることに注意してください。experimental、つまりこの機能は現時点では試験的に実装されている機能であり、正式にリリースされる場合はコードパターンが変更になっている可能性があります。

(※後述しますが、グラフデータベースを検索するためのretrieverは現時点では用意されておらず、サンプルコードから自分で定義する必要があります。正式リリースされるとこのあたりは大幅に改善されているのではないかと期待したいです。)

```python
from langchain_core.runnables import (RunnableBranch, RunnableLambda, RunnableParallel,RunnablePassthrough)
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.prompts.prompt import PromptTemplate
from pydantic import BaseModel, Field

from typing import Tuple, List, Optional
from langchain_core.messages import AIMessage, HumanMessage
from langchain_core.output_parsers import StrOutputParser
import os
from langchain_community.graphs import Neo4jGraph
#from langchain.document_loaders import WikipediaLoader

from langchain.text_splitter import TokenTextSplitter
from langchain.text_splitter import CharacterTextSplitter
from langchain.document_loaders import TextLoader

from langchain_openai import ChatOpenAI
from langchain_experimental.graph_transformers import LLMGraphTransformer
from neo4j import GraphDatabase
from yfiles_jupyter_graphs import GraphWidget
from langchain_community.vectorstores import Neo4jVector
from langchain_openai import OpenAIEmbeddings
from langchain_community.vectorstores.neo4j_vector import remove_lucene_chars
from langchain_core.runnables import ConfigurableField, RunnableParallel, RunnablePassthrough
```

OpenAI、Neo4j、LangSmithのAPIキーを定義します。  
(LangSmithはオプションのため必須ではありません。)

```python
import getpass
import os

def _set_if_undefined(var: str):
    if not os.environ.get(var):
        os.environ[var] = getpass.getpass(f"Please provide your {var}")

# 環境変数の設定
_set_if_undefined("OPENAI_API_KEY")
_set_if_undefined("LANGCHAIN_API_KEY")
_set_if_undefined("NEO4J_USERNAME")
_set_if_undefined("NEO4J_PASSWORD")
_set_if_undefined("NEO4J_AURA_INSTANCEID")
_set_if_undefined("AURA_INSTANCENAME")

# (オプション)LangSmithを使う場合
os.environ["LANGCHAIN_TRACING_V2"] = "true"
os.environ["LANGCHAIN_PROJECT"] = "Graph RAG"
```

neo4jのサイトでグラフデータベースのインスタンスを作成すると以下の認証情報が取得できます。この情報を使ってneo4jに接続するイメージです。(下記はサンプルで架空の情報です。)

- NEO4J\_URI=neo4j+s://258dbas4c.databases.neo4j.io
- NEO4J\_USERNAME=neo4j
- NEO4J\_PASSWORD=KKyeD-Acgzn8kpOQycPgzgfjxxxxxxxxxxxxxxx
- AURA\_INSTANCEID=2fdajkldaa4c
- AURA\_INSTANCENAME=Instance01

#### ドキュメントデータの作成

次にドキュメントデータを作ります。今回はローカルに保存した「サザエさん.txt」のファイルを使います。 [wikipediaのサザエさんのページ](https://ja.wikipedia.org/wiki/%E3%82%B5%E3%82%B6%E3%82%A8%E3%81%95%E3%82%93) からコピペしたテキストファイルです。下記コードではコメントアウトしていますが、langchainのwikipedialoaderを使ってデータセットを作っても問題ありません。

```python
# Read the wikipedia article
# raw_documents = WikipediaLoader(query="サザエさん").load()
raw_documents = TextLoader('サザエさん2.txt').load()
```

テキストデータをチャンクテキストに区切ります。今回はシンプルにlangchainのTokenTextSplitterを使います。

```python
text_splitter = TokenTextSplitter(chunk_size=512, chunk_overlap=24)
documents = text_splitter.split_documents(raw_documents[:3])
```

内容を確認します。

```python
print(documents)
```

以下のように7つのチャンクテキストが確認できます。

```python
[Document(page_content='「サザエさん」に登場する磯野家の主要な人物について、性格や年齢、続柄、周囲の評判などを詳しく説明します。\n\nまず、家長である磯野波平は真面目で厳格な性格の持ち主で、伝統や礼儀を重んじる古風な人物です。彼は約54歳で、家族を大切にする温かい心も併せ持っています。波平は家族から頼りになる父親として信頼されており、近所や職場の同僚からも尊敬されていますが、その厳しさから時折反発を受けることもあります。\n\n波平の妻である磯野フネは、穏やかで慈愛に満ちた性格の持ち主で、家族全員を優しく見守る存在です。彼女は約50歳で、しっかり者として家事全般をこなしています。フネは家族から「お母さん」として慕われ、地域でも「理想の母親」として尊敬されています。困ったときにはフネに相談する人も多', metadata={'source': 'サザエさん2.txt', 'id': '4ab499f134682b55a3ee1263a470da03'}),
 Document(page_content='す。困ったときにはフネに相談する人も多いです。\n\n波平とフネの息子である磯野カツオは、明るく元気で好奇心旺盛な少年です。彼は11歳で、いたずら好きですが素直で憎めない性格です。カツオは家族や友達から愛されていますが、そのいたずらや行動には時折手を焼かれることもあります。学校ではやんちゃな生徒として先生からも知られています。\n\nカツオの妹である磯野ワカメは、素直でしっかり者の女の子です。彼女は9歳で、おっとりした性格で兄のカツオとは対照的です。ワカメは家族から「しっかり者のお姉ちゃん」として認識され、友達からも信頼されています。学校でも模範的な生徒として評価されています。\n\n波平とフネの孫である磯野タラオは、天真爛漫で純粋無垢な性格の持ち主です。彼は3歳で、家族全員に愛される存在です。タラオは近所の人々からも「可愛い子供」として知ら', metadata={'source': 'サザエさん2.txt', 'id': 'df06a0fa7f3bc902764a2554991867ac'}),
 Document(page_content='人々からも「可愛い子供」として知られています。時折いたずらをすることもありますが、その無邪気さで許されることが多いです。\n\n波平とフネの長女であり、タラオの母親であるフグ田サザエは、明るくおおらかで時におっちょこちょいな性格です。彼女は24歳で、正義感が強く、家族を支えるしっかり者の妻であり母親です。サザエは家族から頼りにされ、近所の人々からも「明るい奥さん」として親しまれています。時折の失敗も彼女の魅力の一部とされています。\n\nサザエの夫であり、タラオの父親であるフグ田マスオは、温厚で誠実な性格の持ち主です。彼は28歳で、少し頼りないところもありますが、家族を大切にする優しい父親です。マスオは家族や職場の同僚から信頼され、温かい人柄が評価されています。時折の失敗やドジも彼の人間らしさとして受け入れられています。\n\nこのように、磯野家は個性豊', metadata={'source': 'サザエさん2.txt', 'id': '438d6cd5034edf7a5cf2bc0b2757362c'}),
 Document(page_content='\nこのように、磯野家は個性豊かなキャラクターたちが集まり、家族愛とユーモアに満ちた生活を送っています。それぞれのキャラクターが持つ特徴が物語に深みを与え、多くの視聴者から愛されています。\n\n磯野カツオが通う学校の友達について、彼らの性格や家族、周囲の評判を詳しく紹介します。カツオと彼の友達は、それぞれ個性的で物語に深みと楽しさを加えています。\n\nまず、カツオの親友である中島弘は、明るく社交的な性格です。彼は少しお調子者な一面もありますが、困ったときには頼りになる存在です。中島の家族については、父親の中島一郎と母親の中島恵が登場します。彼は家族とともにカツオと行動を共にすることが多く、二人の仲の良さは学校でも有名です。先生やクラスメートからも友好的で親しみやすい子として知られています。\n\n次に、花沢花子は', metadata={'source': 'サザエさん2.txt', 'id': '1bf43493b810053dbc38278026eac07b'}),
 Document(page_content='�られています。\n\n次に、花沢花子は活発でしっかり者の女の子です。時に少しおせっかいな一面もありますが、カツオに対しては特別な思いを持っている様子があります。花沢家は父親の花沢武と母親の花沢由美で構成されています。クラスメートからはリーダーシップがあり頼りにされることが多く、カツオとはよく口論になりますが、互いを大切に思う気持ちが垣間見えます。\n\nまた、早川という女の子は落ち着いていて冷静な性格です。彼女は勉強が得意で、カツオの相談相手になることもあります。早川家については、父親の早川浩一と母親の早川美奈子が登場します。クラスメートからは「賢い子」として尊敬され、カツオからも頼りにされています。おっとりとした性格で、友達からも慕われています。\n\nさらに、伊佐坂という男の子は真面目で少し内気な性格です。彼は読書が趣味で、', metadata={'source': 'サザエさん2.txt', 'id': 'dff0779d1a1afe43c444aacc04ca64b7'}),
 Document(page_content='な性格です。彼は読書が趣味で、物静かですが友情に厚い一面も持っています。伊佐坂家は、父親の伊佐坂隆と母親の伊佐坂京子がいます。クラスメートからは少し地味な存在として見られることもありますが、知識が豊富で頼りにされることも多いです。カツオとは異なる性格ながらも、良き友人関係を築いています。\n\n最後に、かおりちゃんは優しくて可愛らしい性格の女の子です。カツオが密かに憧れている存在で、クラスの人気者です。彼女の家族については、父親のかおりの父と母親のかおりの母がいます。クラスメートからは「天使のような子」として人気があり、カツオに対しても友好的で、彼のことを気にかけている様子が見られます。\n\nこのように、カツオとその友達たちは、それぞれ個性的で魅力的なキャラクターです。彼らは学校生活を共に過ごし、様々なエピソードを通じて友情を深めています', metadata={'source': 'サザエさん2.txt', 'id': '645abe91e7f4f0207fd1b4348af7132a'}),
 Document(page_content='�ソードを通じて友情を深めています。彼らの関係性と個々の特徴が、物語にさらに彩りを加え、多くの視聴者を楽しませています。', metadata={'source': 'サザエさん2.txt', 'id': 'd915e64f9e7ec3269e46ad9ef11202c8'})]
```

#### グラフデータモデルを抽出

次に、このチャンクテキストからLLMを使って、グラフデータモデルを抽出します。まずは、LLMを定義し、そのLLMを使って、LLMGraphTransformerのconvert\_to\_graph\_documents関数により、上述したデータセットからグラフ構造を抽出します。この実装の一番重要な部分です。

Extraction(Structured Output)の章でも紹介した通り、LLMはテキストデータという非構造化データを理解できるモデルです。なのでこの非構造化データ(テキストデータ)から構造化データ(グラフデータ)を抽出するのにLLMが適しているということです。逆にETLツールのようなルールベースのツールではこのような処理は非常に難しいということになります。

```python
llm=ChatOpenAI(temperature=0, model_name="gpt-3.5-turbo") 
llm_transformer = LLMGraphTransformer(llm=llm)
graph_documents = llm_transformer.convert_to_graph_documents(documents)
```

上記によりこれにより、テキストデータからグラフデータ(つまりグラフのノードとエッジに相当するオブジェクトが抽出できたことになります。出来上がったグラフデータ構造の中身を確認します。

```python
print(graph_documents)
```

以下のように、7つの各チャンクテキストの内容に沿ってNodeとEdgeが抽出されていることがわかります。

```python
[GraphDocument(nodes=[Node(id='磯野波平', type='Person'), Node(id='磯野フネ', type='Person')], relationships=[Relationship(source=Node(id='磯野波平', type='Person'), target=Node(id='磯野フネ', type='Person'), type='SPOUSE')], source=Document(page_content='「サザエさん」に登場する磯野家の主要な人物について、性格や年齢、続柄、周囲の評判などを詳しく説明します。\n\nまず、家長である磯野波平は真面目で厳格な性格の持ち主で、伝統や礼儀を重んじる古風な人物です。彼は約54歳で、家族を大切にする温かい心も併せ持っています。波平は家族から頼りになる父親として信頼されており、近所や職場の同僚からも尊敬されていますが、その厳しさから時折反発を受けることもあります。\n\n波平の妻である磯野フネは、穏やかで慈愛に満ちた性格の持ち主で、家族全員を優しく見守る存在です。彼女は約50歳で、しっかり者として家事全般をこなしています。フネは家族から「お母さん」として慕われ、地域でも「理想の母親」として尊敬されています。困ったときにはフネに相談する人も多', metadata={'source': 'サザエさん2.txt', 'id': '4ab499f134682b55a3ee1263a470da03'})),
 GraphDocument(nodes=[Node(id='磯野カツオ', type='Person'), Node(id='磯野ワカメ', type='Person'), Node(id='磯野タラオ', type='Person')], relationships=[Relationship(source=Node(id='磯野カツオ', type='Person'), target=Node(id='磯野', type='Family'), type='CHILD'), Relationship(source=Node(id='磯野ワカメ', type='Person'), target=Node(id='磯野', type='Family'), type='CHILD'), Relationship(source=Node(id='磯野タラオ', type='Person'), target=Node(id='磯野', type='Family'), type='GRANDCHILD')], source=Document(page_content='す。困ったときにはフネに相談する人も多いです。\n\n波平とフネの息子である磯野カツオは、明るく元気で好奇心旺盛な少年です。彼は11歳で、いたずら好きですが素直で憎めない性格です。カツオは家族や友達から愛されていますが、そのいたずらや行動には時折手を焼かれることもあります。学校ではやんちゃな生徒として先生からも知られています。\n\nカツオの妹である磯野ワカメは、素直でしっかり者の女の子です。彼女は9歳で、おっとりした性格で兄のカツオとは対照的です。ワカメは家族から「しっかり者のお姉ちゃん」として認識され、友達からも信頼されています。学校でも模範的な生徒として評価されています。\n\n波平とフネの孫である磯野タラオは、天真爛漫で純粋無垢な性格の持ち主です。彼は3歳で、家族全員に愛される存在です。タラオは近所の人々からも「可愛い子供」として知ら', metadata={'source': 'サザエさん2.txt', 'id': 'df06a0fa7f3bc902764a2554991867ac'})),
 GraphDocument(nodes=[Node(id='サザエ', type='Person'), Node(id='マスオ', type='Person')], relationships=[Relationship(source=Node(id='サザエ', type='Person'), target=Node(id='タラオ', type='Person'), type='PARENT'), Relationship(source=Node(id='サザエ', type='Person'), target=Node(id='フグ田', type='Person'), type='SPOUSE'), Relationship(source=Node(id='マスオ', type='Person'), target=Node(id='タラオ', type='Person'), type='PARENT'), Relationship(source=Node(id='マスオ', type='Person'), target=Node(id='フグ田', type='Person'), type='SPOUSE')], source=Document(page_content='人々からも「可愛い子供」として知られています。時折いたずらをすることもありますが、その無邪気さで許されることが多いです。\n\n波平とフネの長女であり、タラオの母親であるフグ田サザエは、明るくおおらかで時におっちょこちょいな性格です。彼女は24歳で、正義感が強く、家族を支えるしっかり者の妻であり母親です。サザエは家族から頼りにされ、近所の人々からも「明るい奥さん」として親しまれています。時折の失敗も彼女の魅力の一部とされています。\n\nサザエの夫であり、タラオの父親であるフグ田マスオは、温厚で誠実な性格の持ち主です。彼は28歳で、少し頼りないところもありますが、家族を大切にする優しい父親です。マスオは家族や職場の同僚から信頼され、温かい人柄が評価されています。時折の失敗やドジも彼の人間らしさとして受け入れられています。\n\nこのように、磯野家は個性豊', metadata={'source': 'サザエさん2.txt', 'id': '438d6cd5034edf7a5cf2bc0b2757362c'})),
 GraphDocument(nodes=[Node(id='磯野家', type='Family'), Node(id='磯野カツオ', type='Person'), Node(id='中島弘', type='Person'), Node(id='中島一郎', type='Person'), Node(id='中島恵', type='Person')], relationships=[Relationship(source=Node(id='磯野家', type='Family'), target=Node(id='磯野カツオ', type='Person'), type='CHILD'), Relationship(source=Node(id='磯野カツオ', type='Person'), target=Node(id='中島弘', type='Person'), type='FRIEND'), Relationship(source=Node(id='中島弘', type='Person'), target=Node(id='中島一郎', type='Person'), type='PARENT'), Relationship(source=Node(id='中島弘', type='Person'), target=Node(id='中島恵', type='Person'), type='PARENT')], source=Document(page_content='\nこのように、磯野家は個性豊かなキャラクターたちが集まり、家族愛とユーモアに満ちた生活を送っています。それぞれのキャラクターが持つ特徴が物語に深みを与え、多くの視聴者から愛されています。\n\n磯野カツオが通う学校の友達について、彼らの性格や家族、周囲の評判を詳しく紹介します。カツオと彼の友達は、それぞれ個性的で物語に深みと楽しさを加えています。\n\nまず、カツオの親友である中島弘は、明るく社交的な性格です。彼は少しお調子者な一面もありますが、困ったときには頼りになる存在です。中島の家族については、父親の中島一郎と母親の中島恵が登場します。彼は家族とともにカツオと行動を共にすることが多く、二人の仲の良さは学校でも有名です。先生やクラスメートからも友好的で親しみやすい子として知られています。\n\n次に、花沢花子は', metadata={'source': 'サザエさん2.txt', 'id': '1bf43493b810053dbc38278026eac07b'})),
 GraphDocument(nodes=[Node(id='花沢花子', type='Person'), Node(id='花沢武', type='Person'), Node(id='花沢由美', type='Person'), Node(id='カツオ', type='Person'), Node(id='早川', type='Person'), Node(id='早川浩一', type='Person'), Node(id='早川美奈子', type='Person'), Node(id='伊佐坂', type='Person')], relationships=[Relationship(source=Node(id='花沢花子', type='Person'), target=Node(id='花沢武', type='Person'), type='PARENT'), Relationship(source=Node(id='花沢花子', type='Person'), target=Node(id='花沢由美', type='Person'), type='PARENT'), Relationship(source=Node(id='花沢花子', type='Person'), target=Node(id='カツオ', type='Person'), type='SPECIAL_FEELINGS'), Relationship(source=Node(id='早川', type='Person'), target=Node(id='早川浩一', type='Person'), type='PARENT'), Relationship(source=Node(id='早川', type='Person'), target=Node(id='早川美奈子', type='Person'), type='PARENT'), Relationship(source=Node(id='早川', type='Person'), target=Node(id='カツオ', type='Person'), type='RELIED_ON')], source=Document(page_content='�られています。\n\n次に、花沢花子は活発でしっかり者の女の子です。時に少しおせっかいな一面もありますが、カツオに対しては特別な思いを持っている様子があります。花沢家は父親の花沢武と母親の花沢由美で構成されています。クラスメートからはリーダーシップがあり頼りにされることが多く、カツオとはよく口論になりますが、互いを大切に思う気持ちが垣間見えます。\n\nまた、早川という女の子は落ち着いていて冷静な性格です。彼女は勉強が得意で、カツオの相談相手になることもあります。早川家については、父親の早川浩一と母親の早川美奈子が登場します。クラスメートからは「賢い子」として尊敬され、カツオからも頼りにされています。おっとりとした性格で、友達からも慕われています。\n\nさらに、伊佐坂という男の子は真面目で少し内気な性格です。彼は読書が趣味で、', metadata={'source': 'サザエさん2.txt', 'id': 'dff0779d1a1afe43c444aacc04ca64b7'})),
 GraphDocument(nodes=[Node(id='伊佐坂隆', type='Person'), Node(id='伊佐坂京子', type='Person'), Node(id='カツオ', type='Person'), Node(id='かおりちゃん', type='Person'), Node(id='かおりの父', type='Person'), Node(id='かおりの母', type='Person')], relationships=[Relationship(source=Node(id='伊佐坂隆', type='Person'), target=Node(id='伊佐坂京子', type='Person'), type='SPOUSE'), Relationship(source=Node(id='カツオ', type='Person'), target=Node(id='かおりちゃん', type='Person'), type='FRIEND'), Relationship(source=Node(id='かおりちゃん', type='Person'), target=Node(id='かおりの父', type='Person'), type='PARENT'), Relationship(source=Node(id='かおりちゃん', type='Person'), target=Node(id='かおりの母', type='Person'), type='PARENT')], source=Document(page_content='な性格です。彼は読書が趣味で、物静かですが友情に厚い一面も持っています。伊佐坂家は、父親の伊佐坂隆と母親の伊佐坂京子がいます。クラスメートからは少し地味な存在として見られることもありますが、知識が豊富で頼りにされることも多いです。カツオとは異なる性格ながらも、良き友人関係を築いています。\n\n最後に、かおりちゃんは優しくて可愛らしい性格の女の子です。カツオが密かに憧れている存在で、クラスの人気者です。彼女の家族については、父親のかおりの父と母親のかおりの母がいます。クラスメートからは「天使のような子」として人気があり、カツオに対しても友好的で、彼のことを気にかけている様子が見られます。\n\nこのように、カツオとその友達たちは、それぞれ個性的で魅力的なキャラクターです。彼らは学校生活を共に過ごし、様々なエピソードを通じて友情を深めています', metadata={'source': 'サザエさん2.txt', 'id': '645abe91e7f4f0207fd1b4348af7132a'})),
 GraphDocument(nodes=[Node(id='友情', type='Concept'), Node(id='関係性', type='Concept'), Node(id='特徴', type='Concept'), Node(id='物語', type='Concept'), Node(id='視聴者', type='Concept')], relationships=[Relationship(source=Node(id='友情', type='Concept'), target=Node(id='関係性', type='Concept'), type='DEEPENS'), Relationship(source=Node(id='関係性', type='Concept'), target=Node(id='特徴', type='Concept'), type='ADDS_COLOR'), Relationship(source=Node(id='特徴', type='Concept'), target=Node(id='物語', type='Concept'), type='ADDS_FLAVOR'), Relationship(source=Node(id='物語', type='Concept'), target=Node(id='視聴者', type='Concept'), type='ENTERTAINS')], source=Document(page_content='�ソードを通じて友情を深めています。彼らの関係性と個々の特徴が、物語にさらに彩りを加え、多くの視聴者を楽しませています。', metadata={'source': 'サザエさん2.txt', 'id': 'd915e64f9e7ec3269e46ad9ef11202c8'}))]
```

一つ目のチャンクはこちら。

```python
graph_documents[0]
```

```python
GraphDocument(nodes=[Node(id='磯野波平', type='Person'), Node(id='磯野フネ', type='Person')], relationships=[Relationship(source=Node(id='磯野波平', type='Person'), target=Node(id='磯野フネ', type='Person'), type='SPOUSE')], source=Document(page_content='「サザエさん」に登場する磯野家の主要な人物について、性格や年齢、続柄、周囲の評判などを詳しく説明します。\n\nまず、家長である磯野波平は真面目で厳格な性格の持ち主で、伝統や礼儀を重んじる古風な人物です。彼は約54歳で、家族を大切にする温かい心も併せ持っています。波平は家族から頼りになる父親として信頼されており、近所や職場の同僚からも尊敬されていますが、その厳しさから時折反発を受けることもあります。\n\n波平の妻である磯野フネは、穏やかで慈愛に満ちた性格の持ち主で、家族全員を優しく見守る存在です。彼女は約50歳で、しっかり者として家事全般をこなしています。フネは家族から「お母さん」として慕われ、地域でも「理想の母親」として尊敬されています。困ったときにはフネに相談する人も多', metadata={'source': 'サザエさん2.txt'}))
```

このチャンクから抽出されたNodeがこちら。

```python
graph_documents[0].nodes
```

Nodeとして「磯野波平」と「磯野フネ」が抽出されています。NodeのタイプはPerson、つまり人物だと認識されていることがわかります。

```python
[Node(id='磯野波平', type='Person'), Node(id='磯野フネ', type='Person')]
```

そしてNeo4jではグラフのEdgeに相当するものがrelationshipsになりますのでこちらも内容を確認してみます。

```python
graph_documents[0].relationships
```

「磯野波平」と「磯野フネ」という二つのNodeに対するEdgeに「SPOUSE」、つまり「配偶者」というエンティティが割り当てられています。このチャンクテキスト内のNodeとEdgeが正確に抽出されていることが分かります。

```python
[Relationship(source=Node(id='磯野波平', type='Person'), target=Node(id='磯野フネ', type='Person'), type='SPOUSE')]
```

後の処理で、質問が入力されグラフ検索が行われる際には、この抽出されたNodeとEdgeがグラフ検索の検索キーになるということになります。質問文章から抽出された検索キーで、このグラフデータを検索するというイメージです。

次に、チャンクテキストを確認します。

```python
graph_documents[0].source
```

下記のチャンクテキストのデータがキーワード検索とベクトル検索に使われます。もちろんテキストのままではベクトル検索できませんので後の工程でベクトルに変換する処理を行います。

```python
Document(page_content='「サザエさん」に登場する磯野家の主要な人物について、性格や年齢、続柄、周囲の評判などを詳しく説明します。\n\nまず、家長である磯野波平は真面目で厳格な性格の持ち主で、伝統や礼儀を重んじる古風な人物です。彼は約54歳で、家族を大切にする温かい心も併せ持っています。波平は家族から頼りになる父親として信頼されており、近所や職場の同僚からも尊敬されていますが、その厳しさから時折反発を受けることもあります。\n\n波平の妻である磯野フネは、穏やかで慈愛に満ちた性格の持ち主で、家族全員を優しく見守る存在です。彼女は約50歳で、しっかり者として家事全般をこなしています。フネは家族から「お母さん」として慕われ、地域でも「理想の母親」として尊敬されています。困ったときにはフネに相談する人も多', metadata={'source': 'サザエさん2.txt'})
```

関数一つでテキストデータからグラフデータを自動的に抽出してくれるconvert\_to\_graph\_documentsは非常に便利だと感じます。現時点ではまだexperimentalなので精度はあまり高くないように思いますがこれからどんどん改良されていいくと思います。

#### グラフデータベース(Neo4j)にデータをロードする

ここまでの処理でグラフデータが出来上がっていますので、このデータを下記コードでグラフデータベース(Neo4j)にロードします。

```python
graph = Neo4jGraph()
graph.add_graph_documents(
    graph_documents,
    baseEntityLabel=True,
    include_source=True
)
```

グラフデータベース(Neo4j)にロードできましたのでNeo4jのコンソールから一部を確認してみます。

Neo4jにログインしてNodeとEdge(relationships)を選択すると下記のように目的のグラフデータが参照できます。下記のようにチャンクテキストも確認できます。ノートブックでグラフ構造をチャート化して表示したい場合は下記のようにコードを実行します。

```python
# directly show the graph resulting from the given Cypher query
default_cypher = "MATCH (s)-[r:!MENTIONS]->(t) RETURN s,r,t LIMIT 50"

def showGraph(cypher: str = default_cypher):
    # create a neo4j session to run queries
    driver = GraphDatabase.driver(
        uri = os.environ["NEO4J_URI"],
        auth = (os.environ["NEO4J_USERNAME"],
                os.environ["NEO4J_PASSWORD"]))
    session = driver.session()
    widget = GraphWidget(graph = session.run(cypher).graph())
    widget.node_label_mapping = 'id'
    #display(widget)
    return widget

showGraph()
```

ノートブックでも最低限のチャートで確認が可能です。ここから下記コードでチャンクテキストをベクトル化します。search\_typeが"hybrid"になっていることに注意してください。これはキーワード検索とベクトル検索のハイブリッド検索ということです。

```python
vector_index = Neo4jVector.from_existing_graph(
    OpenAIEmbeddings(model="text-embedding-ada-002"),
    search_type="hybrid",
    node_label="Document",
    text_node_properties=["text"],
    embedding_node_property="embedding"
)
```

ロードしたグラフデータにインデックスを作成します。今回のデータ量ではインデックスを作成するまでもないのですがお作法として。

```python
graph.query("CREATE FULLTEXT INDEX entity IF NOT EXISTS FOR (e:__Entity__) ON EACH [e.id]")
```

ここまでで、グラフデータベースへのデータのロードが完了しました。

#### 検索の仕組みを定義

ここから質問文を入力した際の検索の仕組みを定義してゆきます。

グラフデータベースを検索する際には、当然、質問文章からグラフの検索キーになるキーワードを抽出する必要があります。以降のコードはその定義となります。入力文章からグラフのNodeやEdgeとなるエンティティを抽出するコードです。

```python
# Extract entities from text
class Entities(BaseModel):
    """エンティティに関する情報の識別"""

    names: List[str] = Field(
        ...,
        description="文章の中に登場する、人物、各人物の性格、各人物間の続柄、各人物が所属する組織、各人物の家族関係",
    )
```

次に、指示文を含んだプロンプトテンプレートを定義します。

```python
prompt = ChatPromptTemplate.from_messages(
    [
        (
            "system",
            "テキストから家族と人物のエンティティを抽出します。",
        ),
        (
            "human",
            "指定された形式を使用して、以下から情報を抽出します。"
            "input: {question}",
        ),
    ]
)
```

下記コードで、質問文の入力に対する、LLMの返答(＝抽出されたエンティティ)をEntities関数で指定した構造に変換しプロンプトテンプレートに渡すチェーンを定義しています。

```python
entity_chain = prompt | llm.with_structured_output(Entities)
```

これで質問文からグラフ検索を実行するための検索キーを抽出する定義が完了しました。実行してみます。

```python
entity_chain.invoke({"question": "カツオの両親は誰ですか？"}).names
```

返答は以下の通り。上記の質問文章からグラフデータベースを検索する際に、「カツオ」や\[両親\]というエンティティを検索すればよいということがわかります。

```python
['カツオ', '両親']
```

こちらの出力結果は「カツオの両親は誰ですか？」という質問に対する回答文ではないことに注意してください。この出力はこの質問文が入力されたあとグラフ検索を行うための検索キーを抽出するという処理だということを思い出してください。

つまりこの例では、この質問文に対しては、グラフデータベースの「カツオ」というノードを検索すればグラフのEdge(Rerationship)からカツオのお母さんが誰なのかを検索することができるという、期待通りの出力となります。

これで質問文から検索キーを抽出することができることを確認できましたので、この検索キーを使って実際のグラフ検索を実行する定義が以降のコードになります。

まず、多少のスペルミスを許容する全文検索のクエリを生成するヘルパー関数を下記で定義しています。全文検索エンジンにはApache luceneを使っています。

(※以降のコードをみていただくとわかる通り、これ以降の処理はまだ抽象度の高い関数は用意されていない印象です。)

```python
def generate_full_text_query(input: str) -> str:
    """
    指定された入力文字列に対する全文検索クエリを生成します。

    この関数は、全文検索に適したクエリ文字列を構築します。
    入力文字列を単語に分割し、
    各単語に対する類似性のしきい値 (変更された最大 2 文字) を結合します。
    AND 演算子を使用してそれらを演算します。ユーザーの質問からエンティティをマッピングするのに役立ちます
    データベースの値と一致しており、多少のスペルミスは許容されます。
    """
    full_text_query = ""
    words = [el for el in remove_lucene_chars(input).split() if el]
    for word in words[:-1]:
        full_text_query += f" {word}~2 AND"
    full_text_query += f" {words[-1]}~2"
    return full_text_query.strip()
```

次に、質問文のエンティティを検出、それを反復処理し、Cypherテンプレートを使用して関連するノードの近傍を取得する関数(つまりグラフ検索用のretriever)を定義します。

要はLangChain側でまだグラフ検索用のretriever関数が開発されておらず、現時点ではCypherクエリをベタ書きでretriever関数を自分で定義しているという状態だと思います。

```python
def structured_retriever(question: str) -> str:
    """
    質問の中で言及されたエンティティの近傍を収集します。
    """
    result = ""
    entities = entity_chain.invoke({"question": question})
    for entity in entities.names:
        response = graph.query(
            """CALL db.index.fulltext.queryNodes('entity', $query, {limit:2})
            YIELD node,score
            CALL {
              WITH node
              MATCH (node)-[r:!MENTIONS]->(neighbor)
              RETURN node.id + ' - ' + type(r) + ' -> ' + neighbor.id AS output
              UNION ALL
              WITH node
              MATCH (node)<-[r:!MENTIONS]-(neighbor)
              RETURN neighbor.id + ' - ' + type(r) + ' -> ' +  node.id AS output
            }
            RETURN output LIMIT 50
            """,
            {"query": generate_full_text_query(entity)},
        )
        result += "\n".join([el['output'] for el in response])
    return result
```

グラフ検索のretrieverが定義できましたので、実行してみます。

```python
print(structured_retriever("カツオの両親は誰ですか？"))
```

下記の通り、グラフ検索により、カツオのNodeとPARENTのrelationshipsの関係にある「波平」と「フネ」のNode検索ができています。

```python
花沢花子 - SPECIAL_FEELING -> カツオ
早川 - RELIANCE -> カツオ
磯野カツオ - FRIEND -> 中島弘
波平 - PARENT -> 磯野カツオ
フネ - PARENT -> 磯野カツオ
磯野家 - CHILD -> 磯野カツオ磯野波平 - SPOUSE -> 磯野フネ
磯野波平 - PARENT -> 磯野フネ
磯野波平 - SPOUSE -> 磯野フネ
磯野波平 - PARENT -> 磯野フネ
```

このような簡単なデータセットで簡単な質問の場合はベクトル検索だけも十分にちゃんとした回答を生成できますが、仮にベクトル検索が有効でなかった場合でも、上記のようにグラフ検索のほうで得られたコンテキストで回答できるということがわかります。

ここから最終的なretriever関数を定義します。冒頭で説明した通り、GraphRAGではグラフ検索、全文検索だけではなく、ベクトル検索も実行します。そのため、上記で定義したグラフ検索用のstructured\_retrieverにベクトル検索用のsimilarity\_searchを合体させて最終的なretrieverを下記コードで定義しています。

```python
def retriever(question: str):
    print(f"Search query: {question}")
    structured_data = structured_retriever(question)
    unstructured_data = [el.page_content for el in vector_index.similarity_search(question)]
    final_data = f"""Structured data:
{structured_data}
Unstructured data:
{"#Document ". join(unstructured_data)}
    """
    return final_data
```

そして、最終的な応答を得るためのプロンプトテンプレートを定義します。

```python
template = """あなたは優秀なAIです。下記のコンテキストを利用してユーザーの質問に丁寧に答えてください。
必ず文脈からわかる情報のみを使用して回答を生成してください。
{context}

ユーザーの質問: {question}
"""

prompt = ChatPromptTemplate.from_template(template)
```

次に、ここまでロジックを積み上げ定義してきた全てのオブジェクトをチェーンにしてGraphRAGのパイプラインの出来上がりです。

```python
chain = (
    RunnableParallel(
        {
            "context": _search_query | retriever,
            "question": RunnablePassthrough(),
        }
    )
    | prompt
    | llm
    | StrOutputParser()
)
```

質問文章を入力してみます。

```python
chain.invoke({"question": "磯野カツオと一番仲の良い友達は誰ですか？"})
```

出力

```python
Search query: 磯野カツオと一番仲の良い友達は誰ですか？
'磯野カツオと一番仲の良い友達は中島弘です。彼はカツオの親友であり、明るく社交的な性格で、カツオと行動を共にすることが多いです。また、学校でも二人の仲の良さは有名です。'
```

その他、いろいろ質問文章を入力してみます。

```python
Search query: タラちゃんのお母さんは誰ですか？
'タラちゃんのお母さんはフグ田サザエです。'
```

```python
Search query: カツオとタラちゃんの続柄は何ですか？
'磯野カツオと磯野タラオは、叔父と甥の関係です。カツオはタラオの父親の兄です。'
```

```python
Search query: カツオ、タラちゃん、サザエ、マスオの4人の続柄を説明してください。
'カツオとタラオ（タラちゃん）は直接の血縁関係にあります。カツオはタラオの叔父にあたります。サザエはカツオの姉であり、タラオの母親です。マスオはサザエの夫であり、タラオの父親です。したがって、マスオはカツオの義兄にあたります。'
```

```python
Search query: カツオの学校の代表的な友達を挙げてください。
'磯野カツオの学校での代表的な友達には、中島弘、花沢花子、早川、そして伊佐坂がいます。中島弘は明るく社交的で、カツオと非常に仲が良いです。花沢花子は活発でしっかり者で、カツオとは時に口論になることもありますが、特別な思いを持っている様子です。早川は落ち着いていて冷静、勉強が得意でカツオの相談相手になることもあります。伊佐坂は真面目で内気な性格で、読書が趣味です。これらの友達とカツオは学校生活を共に過ごし、様々なエピソードを通じて友情を深めています。'
```

```python
Search query: 中島恵とカツオはどのような関係ですか？
'中島恵は中島弘の母親であり、中島弘は磯野カツオの親友です。したがって、中島恵はカツオの友達の母親という関係になります。また、中島恵の息子である中島弘がカツオと一緒に行動することが多いため、カツオは中島恵の家にも頻繁に訪れる可能性があります。このように、中島恵はカツオにとって親しい友達の母親という位置づけです。'
```

```python
Search query: 中島恵とワカメちゃんはどのような関係ですか？
'中島恵と磯野ワカメは直接的な関係はありません。中島恵は中島弘の母親であり、磯野ワカメは磯野カツオの妹です。ただし、中島弘と磯野カツオは友人関係にあり、そのため中島恵とワカメは間接的に知り合いという関係になる可能性があります。'
```

```python
Search query: 伊佐坂家の中でワカメちゃんと仲の良い人は誰ですか？
'伊佐坂家についての情報では、ワカメちゃんと特に仲が良いとされる人物についての言及はありません。ワカメちゃんは磯野家の一員であり、伊佐坂家との関係についての具体的な詳細は提供されていないため、この質問には正確な答えを出すことができません。'
```

```python
Search query: 早川さんとワカメちゃんはどのような関係ですか？
'早川さんと磯野ワカメちゃんについての直接的な関係は文書からは明確には示されていません。ただし、早川さんがカツオの相談相手であり、ワカメちゃんがカツオの妹であることから、彼らは共通の知人を通じて間接的な関係がある可能性があります。また、早川さんが落ち着いていて冷静な性格であり、ワカメちゃんがおっとりしていて信頼されている点から、学校や友人関係の中で良好な関係を持っている可能性が考えられます。'
```

以上、GraphRAGの解説でした。

[0](#comments)

コメント一覧へ移動

X（Twitter）でシェアする

Facebookでシェアする

はてなブックマークに追加する
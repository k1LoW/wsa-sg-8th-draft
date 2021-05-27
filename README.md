# システムの変化に追従可能でかつ理解し易いドキュメントシステムのモデル化

## 1. モチベーション

- システムの運用をするためにはシステムの理解が必要
- ところが、システムは継続的に開発されるものであり変化するものである。また、その変化のスピードは早くなっている(要出典)
- また、システムは巨大で複雑なものになっており1人で全てを理解するのは現実的に難しくなってきている
- 上記のような状況の中、継続的にシステムを運用していくためのシステム理解の効率的な手法を得たい
    - ゼロからシステムを理解する場合
    - 変化したシステムについて理解の追従をさせる場合

## 2. ターゲット

- 「既にユーザに機能を提供しており運用が必要なシステム」にターゲットを絞る。新規開発などの設計段階のシステムは含まない。
- システムを理解する手法としてドキュメントに注目する


## 3. システムとそれを説明するドキュメントのモデル化

### 前提

- エンジニアがシステムを理解しようとするとき、ドキュメントの内容が正しくシステムを説明している場合、システムを直接見て理解するより、ドキュメントを通じて理解するほうが理解し易いとする
    - つまり、本稿は「いかに正しいドキュメントを得るか」を主題とする

### システム

- あるシステムにおいて、理解しなければならない要素を本稿では「システム要素」と呼ぶ。システム要素をs1, s2, ... snとし、それらを元とした集合 {s1, s2, ... sn} をSとする
    - システム要素は、例えばインフラであればサーバ、コンテナ、ミドルウェア、ネットワークなど

### ドキュメント

- システムを説明するドキュメントを集合Dとする。ドキュメントはシステム要素を説明するものである。
    - 「ドキュメントDがあるシステム要素snを説明できている」場合、関数desc()をつかって desc(sn)∈Dとする

（関数とは集合Aの各要素に集合Bの唯一つの要素を割り当てるもの）

#### 真に正しいドキュメント

desc:S -> D

また

desc(S) = D

（外延性の公理より、集合としては元の重複は考えない）

#### 正しいことが記載されているが不十分なドキュメント

D⊆desc(S)

Dはdesc(S)の部分集合

ある集合Aの要素の個数を記号n(A)で表す。

n(D ∩ desc(S))がn(S)に近ければ近いほど良い

n(S) - n(D ∩ desc(S)) = 0 が理想。

#### 間違ったことが記載されているドキュメント

desc(sx)∈Dかつsx∉Sとなる元sxが存在する

これを本稿では間違い要素と呼ぶ。間違い要素の集合は

D - desc(S) で表すことができる（差集合）

### 時間の経過に応じて変化するシステムとドキュメント

システムは継続的に開発されるものであり変化するものである。

ある時刻tのシステムをStと表す。

時刻tが、t1,t2,...tnと経過する場合、それぞれのシステムをSt1, St2, ... Stnとすると、

簡易的に、システムの巻き戻し（revertなど）を考えないとし

n(St1 ∩ St1) = n(St1)

n(St1 ∩ St2) > n(St1 ∩ St3) > ... > n(St1 ∩ Stn)

と、システム要素の時間の経過に応じて変化していくとする。

この場合、ドキュメントDが仮にD=desc(St1)だとしても、時間の経過と共にn(D ∩ desc(Stn))の数は小さくなる。

これは「正しく説明できているシステム要素」の数が時間の経過に応じて小さくなることを表している。

また間違い要素集合 D - desc(Stn) も存在することになる。

### システムの変化に追従するドキュメント

これはSのシステム要素の変化に応じてDの要素が追従できれば良い。

人が実施するのであれば「システムの変化に応じてドキュメントを書き直せばよい」ということになる。

ただ、近年システムの変化のスピードは早くなっている。またシステムは巨大で複雑なものになっており、手動でシステムに変化に追従するのは難しい。

どこでドキュメントの自動生成が考えられる。

まず、ドキュメントDを生成する関数をfとした場合、時刻によって変化するS自体を入力としたf(S)を作るということがドキュメントの自動生成を表すと言える

D=f(Stn)

さらにその結果が desc(S) となれば、Sの変化に追従できるといえる。

D=f(Stn)=desc(Stn)

つまり「システムの情報からドキュメントを生成する」というアプローチが良さそうということがわかる。

### システムとドキュメントを変化に応じて生成する

今までのモデル化で別のアプローチも表現できる。

あるシステムSは常に設定Cから生成される(gen)ものであった場合、

gen(Ctn) = Stn

また設定Cからドキュメントも生成できれば(doc)、

doc(Ctn) = D

Ctn = doc^-1(D)

> 逆関数があるとする

D = doc(Ctn) = doc(gen^-1(Stn))

となり、

doc(gen^-1()) が f() といえる。

OpenAPIなどがこのアプローチにあたる

## 4. システムの理解のしやすさのモデル化

セクション3では、前提に「ドキュメントを通じてシステムを理解する」と置いている。

また、真に正しいドキュメントとして D = desc(S) とモデル化している。

つまりシステム要素の数とドキュメントの要素の数は同数となる

n(D) = n(S)

近年システムは巨大で複雑なものになっており1人で全てを理解するのは現実的に難しくなってきている。

つまり、人一人が最終的に理解できる要素の数をmとすると

m < n(S)

であるということになる。

また、人一人が一度に理解できる要素の数をkとすると

k < m < n(S)となる

ドキュメントDを理解しやすくするためには、部分集合D1, D2, D3...Dnに分割してそのそれぞれのDnは

k < n(Dn)

であることが望ましいことがわかる。

部分集合の分け方については、さらに理解しやすくするために様々なアプローチが取れる。

例えばサーバ一覧やネットワーク図など、同じようなシステム要素をまとめてグルーピング化しても良いし、

各マイクロサービスごとに分けても良いし、ある処理ごとに分けても良い。

これを ISO/IEC/IEEE 42010:2011 にならって `ビュー` と呼ぶ（ただし本家のそれとは厳密には定義が異なる）。

ビューはステークホルダー（そのドキュメントの対象者）が理解すべきコンテキストに沿って記述される。

ネットワーク管理者にとってはネットワーク図、あるマイクロサービスを管理するSREにとってはそのマイクロサービスを構成するノードとネットワーク、など。

よってD1に含まれる要素がD2に含まれることもある。

よって理解しやすさを持ったドキュメントの特徴としては以下のようになる

D = D1 ∪ D2 ∪ D3 ∪ ... ∪ Dn

## 5. ドキュメントシステムに必要な要件

「継続的にシステムの変化に追従できる」「理解しやすい」ドキュメントシステムの要件として

セクション3より、

D=f(Stn)=desc(Stn)

であること

セクション4より、

D = D1 ∪ D2 ∪ D3 ∪ ... ∪ Dn

というドキュメントが生成できること

と主張した。

ここで、さらにf(S)についてdesc(S)とするために分解すると、

1. 入力Sからシステム要素を列挙すること
2. 1で列挙したそれぞれのシステム要素について理解のための文章を追加すること（本稿ではアノテーションと呼ぶ）

となりそうである。

ここで、2のアノテーションは技術的に困難であるとして人に任せるとすると、1だけが残る。

つまり、

1. 対象システムからシステム要素を抽出でき、それぞれに対して穴埋め形式でドキュメントを記述できる
    - もしくはf^-1(D)=Sを検証できる仕組みを持つ
2. 1で抽出したシステム要素を任意のグループでグルーピングすることができ、それに対してドキュメントを記述できる
    - システム要素はグループ間で重複可
    - 1で抽出した要素のみで2を実現することでs∉Sとなる要素が存在しないように

という要件が見えてくる。

#### ndiag

上記の要件を満たすドキュメンテーションツールとして筆者はndiagというツールを作成している

https://github.com/k1LoW/ndiag

ndiagはシステム要素の抽出機能を持っていないので、例えばノード間のリレーションなどはサービスマップや https://github.com/yuuki/shawk などを入力とするようなことを想定している

## 参考資料

- [目的に沿ったDocumentation as Codeをいかにして実現していくか / PHPerKaigi 2021](https://speakerdeck.com/k1low/phperkaigi-2021)
- [ソフトウェアシステムアーキテクチャ構築の原理 第2版](https://www.sbcr.jp/product/4797376722/)

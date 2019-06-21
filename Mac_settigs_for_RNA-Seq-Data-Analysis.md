## 端末の環境設定（Mac）

### Xcodeをインストールする
App storeからインストールします。

Macの各種ライブラリのインストールに必要なのはxcodeの"Command Line Tools"なのですが、
現在は
[Command Line Toolsだけをインストールすることもできるようです](https://qiita.com/iwaseasahi/items/eb820762600c815ab100)。

iOSの開発はしないからXcodeは必要無いという人はこちらでも良いかもしれません。


### Homebrewをインストールする

macOS用のパッケージマネージャーです。すでにインストールされている人が多いと思われますが、まだであれば
[Homebrewの公式サイト](https://brew.sh/index_ja.html)を見てインストールしてください。

※公式サイトを見て、、なのですがやることは下記のスクリプトをターミナルに貼り付けて実行するだけなので、
公式ページを開くのが面倒であればここからコピペしてもインストールできるはずです。
```markdown
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

※ 今回は必要ありませんが、Bioinfomatics関係のライブラリをインストールする際は、フォーミュラをbrew tapでリポジトリに取り込んで置くと
インストールをスムーズに行うことができる場合があります。
```
$ brew tap brewsci/science
$ brew tap brewsci/bio

```

### gitをインストールする

gitも無ければインストールしてください。今回のワークショップでは、必須ではないかもしれませんが、
このレポジトリにあるドキュメントを手元のマシンにgit cloneしたいなどの需要があれば必要です。

インストールする手段は色々ある様で、例えばCommand line toolsがインストールされていると
gitコマンドを実行しようとするだけでインストールされる様ですが（未確認）、
Homebrew で下記の様にあらかじめインストールしておくのが何も考えないで済み楽な気がします。

```
$ brew install git
```
※ 一台のMacに複数のユーザを設定して使っている場合、homebrewを使うとpermission errorとなるケースがあるようです。
homebrewは現在sudoすることができないため、この場合/usr/local/Cellarをchownする必要があります。詳細は[こちら](https://qiita.com/HIROSHI-1403/items/c90699c1a3f3bd9d63f9)を参考に。

## Dockerのインストール
[公式のSaitoのDLリンク](https://docs.docker.com/docker-for-mac/install/)から
Docker Desktop for MacのインストーラーをDLして、表示される手順通りにインストールを進めます
（dockerhubへのサインインが必要です）。


## minicondaのインストール
[Condaの公式サイト](https://docs.conda.io/en/latest/miniconda.html)から
自分の環境にあったインストーラをDLしてください。

### condaのコマンド
[anaconda のコマンドリストメモ](https://qiita.com/natsuriver/items/4ae6eed5f47e34817090)

### kallistoをインストール

トランスクリプトームのリファレンス配列のインデックス作成と、発現定量にkallistoを利用します。
kallistoは [condaコマンドでインストールすることができます](https://bioconda.github.io/recipes/kallisto/README.html)。

```
$ source ~/.bash_profile
$ conda config --add channels defaults
$ conda config --add channels conda-forge
$ conda config --add channels bioconda
$ conda install kallisto
```

※ kallistoはHomebrewでもインストールすることはできますが、brewsci/scieneをtapして置く必要があります。
詳細は[こちら](https://qiita.com/epsilonminder/items/e3b1fc00edb63cb3a32b)で。


## jupyter, seabornをインストール

- jupyterをインストール
```
conda install jupyter jupyter_console qtconsole notebook sphinx spinxcontrib nbconvert
```
- seabornをinstall(seabornをインストールしてすると、numpy, scipy, pandasなどもついでにインストールされる)
```
conda install seaborn 
```

## CWLを使ってkallistoを実行する

詳細は[こちら](http://bonohu.jp/blog/running-kallisto-via-cwl.html)を確認してください

### cwltoolのインストール

`conda install cwltool`
`git clone https://github.com/pitagora-network/pitagora-cwl`

### SRAをFASTQに変換
```
cd <working directory>
# pitagora-cwlを使ってfasterq-dumpを手元のマシンにインストールすることなく実行
cwltool pitagora-cwl/tools/fasterq-dump/fasterq-dump.cwl \
--srafile SRR7300567.sra

# pigzで生成されたFASTQファイルを圧縮
# condaでpigzをinstall
conda install pigz
pigz SRR7300567.sra*.fastq
```

### kallisto index作成

```
# pitagora-cwlを使ってkallistoを手元のマシンにインストールすることなく実行
cwltool pitagora-cwl/tools/kallisto/index/kallisto_index.cwl \
--fasta_files Homo_sapiens.GRCh38.cdna.all.fa.gz \
--index_name Human
```


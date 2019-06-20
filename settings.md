## Dockerのインストール
[公式のSaitoのDLリンク](https://docs.docker.com/docker-for-mac/install/)から
Docker Desktop for MacのインストーラーをDLして、表示される手順通りにインストールを進めます
（dockerhubへのサインインが必要です）。


## minicondaのインストール
[Condaの公式サイト](https://docs.conda.io/en/latest/miniconda.html)から
自分の環境にあったインストーラをDLしてください。

### condaのコマンド
[anaconda のコマンドリストメモ](https://qiita.com/natsuriver/items/4ae6eed5f47e34817090)

## seaborn

- jupyterをインストール
```
conda install jupyter jupyter_console qtconsole notebook sphinx spinxcontrib nbconvert
```
- seabornをinstall(seabornをインストールしてすると、numpy, scipy, pandasなどもついでにインストールされる)
```
conda install seaborn
```


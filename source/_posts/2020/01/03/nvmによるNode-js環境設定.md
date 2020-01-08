---
title: nvmによるNode.js環境設定
date: 2020-01-03 22:36:21
tags: Node.js
---
# nvmによるNode.js環境設定

## nvmをインストール
Gitでマニュアルインストール

~~~
    export NVM_DIR="$HOME/.nvm" && (
      git clone https://github.com/nvm-sh/nvm.git "$NVM_DIR"
      cd "$NVM_DIR"
      git checkout `git describe --abbrev=0 --tags --match "v[0-9]*" $(git rev-list --tags --max-count=1)`
    ) && \. "$NVM_DIR/nvm.sh"
~~~

ログイン後すぐに使えるように、~/.bashrc, ~/.zshrcなどに以下を追記

~~~
    export NVM_DIR="$HOME/.nvm"
    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
~~~

マニュアルアップデート

~~~
    (
      cd "$NVM_DIR"
      git fetch --tags origin
      git checkout `git describe --abbrev=0 --tags --match "v[0-9]*" $(git rev-list --tags --max-count=1)`
    ) && \. "$NVM_DIR/nvm.sh"
~~~

Terminalを開き直して、以下を実行

~~~
    $ command -v nvm
    nvm
~~~

nvm と表示されればインストール成功

## nodeのインストール

~~~
    # LTS(Long Term Support)バージョンのnodeと最新のnpmをインストール
    $ nvm install --lts --latest-npm

    # defaultの設定
    $ nvm alias default 'lts/*'
~~~

Qiita記事などを見るとないのだが、私の環境では上記のlts/\*には''が必要だった

## その他のコマンド

~~~
    # インストール可能なバージョンを調べる
    $ nvm ls-remote

    # 特定のバージョンをインストール
    $ nvm install x.x.x #例えば x.x.xのところは、12.13.0

    # インストール済みバージョンを表示
    $ nvm ls

    # 特定のバージョンを使用する
    $ nvm use x.x.x

    # 最新のnpmをインストールする
    $ nvm install-latest-npm
~~~

# プロジェクトごとのnode.jsのバージョン管理について

## .nvmrc

プロジェクトのフォルダに.nvmrcファイルを作って中にバージョンを記載しておく

nvm use, nvm install, nvm exec, nvm run, and nvm which などのコマンド実行時に、.nvmrcを見てくれるようになる

~~~
    $ echo "12.13.0" > .nvmrc

    $ echo "lts/*" > .nrmrc # 最新のLTSバージョン

    $ echo "node" > .nrmrc # 最新バージョン
~~~

~~~.nvmrc
  12.13.0
~~~

* [.nvmrc](https://github.com/nvm-sh/nvm#nvmrc)

## ディレクトリごとの便利なバージョン管理方法

.nvmrcファイルがあるディレクトリに移動したときに、自動でnvm useが走るようにターミナルの設定をすることができる

nvm公式サイトに .bashrc と .zshrc のサンプルがある

ここでは、.zshrcのサンプルを引用

~~~.zshrc
# place this after nvm initialization!
autoload -U add-zsh-hook
load-nvmrc() {
  local node_version="$(nvm version)"
  local nvmrc_path="$(nvm_find_nvmrc)"

  if [ -n "$nvmrc_path" ]; then
    local nvmrc_node_version=$(nvm version "$(cat "${nvmrc_path}")")

    if [ "$nvmrc_node_version" = "N/A" ]; then
      nvm install
    elif [ "$nvmrc_node_version" != "$node_version" ]; then
      nvm use
    fi
  elif [ "$node_version" != "$(nvm version default)" ]; then
    echo "Reverting to nvm default version"
    nvm use default
  fi
}
add-zsh-hook chpwd load-nvmrc
load-nvmrc

~~~

* [.zshrc](https://github.com/nvm-sh/nvm#zsh)

# 参考文献

* [Node Version Manager](https://github.com/nvm-sh/nvm#node-version-manager---)
* [nvm(Node Version Manager)を使ってNode.jsをインストールする手順](https://qiita.com/ffggss/items/94f1c4c5d311db2ec71a)


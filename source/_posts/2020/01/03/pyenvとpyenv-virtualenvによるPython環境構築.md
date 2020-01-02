---
title: pyenvとpyenv-virtualenvによるPython環境構築
date: 2020-01-03 00:50:39
tags: Python
---

# pyenv, pyenv-virtualenv cheat sheet


## 仮想環境の作成
・インストール可能なバージョンを確認する

        $ pyenv install --list

・特定バージョンのインストール

        $ sudo pyenv install 3.8.0

・仮想環境の作成(myenvという名前で作成)

        $ sudo pyenv virtualenv 3.6.3 myenv

・作成した環境の確認

        $ pyenv versions
        system
        * 3.8.0 (set by /home/username/.pyenv/version)
        3.8.0/envs/myenv
        myenv

## 仮想環境の適用
・現在のディレクトリに仮想環境を適用<br/>

        $ pyenv local myenv

※実行されたカレントディレクトリに.python-versionというファイルが作成される。

・全環境に適用する

        $ pyenv global myenv

・一時的に仮想環境を適用する

        $ pyenv activate myenv

・一時的に適用した環境を解除する

        $ pyenv deactivate

## 仮想環境をアンインストールする

        $ sudo pyenv uninstall myenv

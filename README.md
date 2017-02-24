# 2017年2月ウラカタ勉強会 Vagrantによるローカル仮想環境の構築

## 使用したスライド

[ウラカタ勉強会 2017年2月度 Vagrantによるローカル仮想環境の構築 // Speaker Deck](https://speakerdeck.com/urakata/urakatamian-qiang-hui-2017nian-2yue-du-vagrantniyorurokarujia-xiang-huan-jing-falsegou-zhu)


## 使用した Vagrantfile
[Vagrantfile](https://github.com/retdaisuke/urakata1702/blob/master/scotchbox/Vagrantfile)

## スライド補足資料
スライドで使用したURLやコマンド、コードをこちらで補足したので参考にしてください。

### インストール


https://www.virtualbox.org/wiki/Downloads

https://www.vagrantup.com/downloads.html


Vagrant のインストールの確認。基本はこちらを利用してください。

    $ vagrant --version

以下でも良いですが、こちらはVagrantfileを読み込んでしまうようで、Vagrantfile
に間違いがあるとエラーがでるので注意してください。

    $ vagrant version

### 仮想マシンの構築

Box の探し方  
https://atlas.hashicorp.com/boxes/search

Scotch Box  
https://box.scotch.io/

作業フォルダの作成してそのフォルダへ移動

    $ mkdir -p ~/urakata1702/scotchbox
    $ cd ~/urakata1702/scotchbox

Vagrantfile を作成する

    $ vagrant init

Vagrantfile を編集して、仮想マシンが使用する BOX とネットワークを設定する


```rb
config.vm.box = "scotch/box"

config.vm.network "private_network", ip: "192.168.33.10"
```

ローカル保存済みの BOX ファイルである scotch.box をVagrant に追加する

    $ vagrant box add --name scotch/box scotch.box


### 仮想マシンの起動と終了、削除

仮想マシンの起動

    $ vagrant up

仮想マシンの状態の確認

    $ vagrant status

仮想マシンの終了

    $ vagrant halt

仮想マシンの削除

    $ vagrant destroy



### 共有フォルダの利用

http://192.168.33.10/


仮想マシンにSSHでログインする

    $ vagrant ssh

仮想マシンの /var/www 以下を確認するが空っぽ (´・ω・\`)

    vagrant@scotchbox:~$ ls /var/www

仮想マシンからログアウト

    vagrant@scotchbox:~$ exit

Vagrantfile に共有フォルダの設定を記述する

```rb
config.vm.synced_folder ".", "/var/www", :mount_options => ["dmode=777", "fmode=666"]
```

Vagrantfile を置いているフォルダに public という名前のフォルダを作成する

    $ mkdir public

public フォルダの中に適当に test と書いた index.html を作成する

    $ echo ’test’ > public/index.html


### Snapshot

スナップショットの作成

    $ vagrant snapshot push

スナップショットの一覧

    $ vagrant snapshot list

スナップショットを復元する

    $ vagrant snapshot pop



### Vagrant Share

Vagrantfile にポート転送の設定を記述する

```rb
config.vm.network "forwarded_port", guest: 80, host: 8080
```

Atlas アカウントの取得URL

https://atlas.hashicorp.com/account/new

Atlasにログインする

    $ vagrant login

Vagrant Shareする

    $ vagrant share

エラーが出る場合は、ポートを指定して Vagrant Shareする

    $ vagrant share --http 8080


### 最後に
ローカルから手動で追加した Scotch Box を削除する

    $ vagrant box remove scotch/box


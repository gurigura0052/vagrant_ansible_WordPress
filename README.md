# ansibleでVagrantに環境を構築し、WordPressをインストールする

- **playbook/gloup_vars/main.yml** を編集する。
```
mysqlの設定
MYSQL:
  USER: root
  PASS: root
  NAME: 作成するデータベース名
WordPressの設定
WORDPRESS:
  NAME: WordPressをインストールするディレクトリ名
  TITLE: サイト名
  LOCALE: 言語設定
  URL: サイトのアドレス
  DB:
    NAME: "{{ MYSQL.NAME }}"
    USER: "{{ MYSQL.NAME }}"
    PASSWORD: "{{ MYSQL.PASS }}"
    HOST: localhost
  ADMIN:
    NAME: ログインユーザー名
    PASSWORD: ログインパスワード
    MAIL_ADDRESS: メールアドレス
```

- **playbook/site.yml** の **webservers.yml** と **db.yml** をコメントインして **wp.yml** をコメントアウトする。

- 新しくデータベースを作らないなら **playbook/roles/db/tasks/main.yml** の **create database** をコメントアウトしておく

- **vagrant up**（一度upした後だったら**vagrant provision**）する。

- **vagrantfile** の **config.vm.synced_folder** を下記に書き換える。
```
config.vm.synced_folder "./var/www", "/var/www", owner: "apache", group: "apache", mount_options: ["dmode=777", "fmode=777"]
```

- ホスト名を設定する場合は下記を **vagrantfile** に記述
```
config.vm.hostname = "ホスト名"
```

- **webservers.yml** と **db.yml** をコメントアウトして **wp.yml** をコメントインする。

- **vagrant reload** した後、**vagrant provision** する。
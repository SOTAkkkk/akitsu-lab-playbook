# akitsu-lab-playbook

## 疎通確認
```shell
$ ansible os3-285-32121.vs.sakura.ne.jp -i inventory -m ping
```

## playbookを当てる
シンタックスチェック
```shell
$ ansible-playbook -i inventory --syntax-check bookApp.yml 
```
dry run
```shell
$ ansible-playbook -i inventory -C bookApp.yml 
```
run
```shell
$ ansible-playbook -i inventory bookApp.yml 
```

## 注意
### mysql-community-serverのインストールが上手くいかない場合がある。
#### 手順
1. `yum clean all`
2. `sudo yum remove mariadb-libs`
3. `sudo rm -rf /var/lib/mysql`
4. `sudo rpm -ivh http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm` 
5. `vim /etc/yum.repos.d/mysql-community.repo` or `yum-config-manager --nogpgcheck local`以下参照(*)
6. `sudo yum install mysql-community-server`


- (*)[mysql+ の Centos 7 yum インストールには以下が必要です: libsasl2.so.2() (64bit)](https://juejin.cn/post/6844904039579123725)を参考に、サーバの/etc/yum.repos.d/mysql-community.repoファイルのgpgcheckを以下のように修正する。
```
gpgcheck=0
```
### DBの構築
- 初期パスワード確認
```shell
$ sudo cat /var/log/mysqld.log | grep password
```
- パスワード変更
```sql
mysqlにログイン後、以下のコマンドを打つ
mysql> set global validate_password_length=4; # 文字列の長さを変更
mysql> set global validate_password_policy=LOW; # ポリシーを変更
mysql> show variables like 'validate_password%';
```
[Mysql 5.7* パスワードをPolicyに合わせるとめんどくさい件について](https://qiita.com/keisukeYamagishi/items/d897e5c52fe9fd8d9273)

[Password Validation Plugin Options and Variables](https://dev.mysql.com/doc/refman/5.7/en/validate-password-options-variables.html)
- DB,table作成
```sql
mysqlにログイン後、以下のコマンドを打つ
$ source bookDBinit.sql
```

### appendix
| 設定項目  |  VPS設定   |
|-------|:--------:|
| OS    | CentOS 7 |
| 開放port |          |
| 公開鍵   |          |
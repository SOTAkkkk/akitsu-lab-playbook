# akitsu-lab-playbook

## 疎通確認
```shell
ansible os3-285-32121.vs.sakura.ne.jp -i inventory -m ping
```

## playbookを当てる
シンタックスチェック
```shell
ansible-playbook -i inventory --syntax-check yum.yml 
```
dry run
```shell
ansible-playbook -i inventory -C yum.yml 

```
run
```shell
ansible-playbook -i inventory yum.yml 
```
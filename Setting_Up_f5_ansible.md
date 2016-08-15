# f5-ansibleの設定の設定

## 検証環境
以下の環境で実施

- ansible
  - CentOS 7.2.1511
  - ansible 2.2.0

### モジュールの取得
F5社のgithubからf5-ansibleを取得します。
※今回、テスト用のディレクトリとしてf5-testを作成し、以降はこのディレクトリ配下で作業を行います。

```
# cd ~/work/
# mkdir f5-test
# git clone https://github.com/F5Networks/f5-ansible
# cp -pr f5-ansible/library ./f5-test/
```

### インベントリファイルの作成

```
# cd ~/work/f5-test
# mkdir inventory
# vi inventory/hosts
localhost ansible_host=127.0.0.1
[bigip]
bigip1 ansible_host=172.23.141.62
```

### BIG-IP接続認証用の変数ファイル作成
認証用のID/パスワードのファイルが平文だと、セキュリティ的に問題があるので、Ansibleの[vault](http://docs.ansible.com/ansible/playbooks_vault.html)の機能で暗号化しておく。

```
# cd ~/work/f5-test
# mkdir -p host_vars/bigip1/
# cd host_vars/bigip1/

# vi main.yml
remote_user: "{{ vault_remote_user }}"
password: "{{ vault_password }}"

# vi vault_vars.yml
vault_remote_user: "testuser1"
vault_password: "password"

# ansible-vault encrypt vault_vars.yml

# cat vault_vars.yml
$ANSIBLE_VAULT;1.1;AES256
3561・・・
```
編集したいときは、
```
# ansible-vault edit vault_vars.yml
Vault password:
```

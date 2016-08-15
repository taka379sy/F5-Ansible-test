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

PlayBookを実行するときは、--ask-vault-passのオプションをつけて、複合キーを入力するようにする。
```
ansible-playbook xxx.yml --ask-vault-pass
```

### 他の接続認証の方法についてのメモ

今回は上記の認証方法で実行しますが、他にも方法があるのでメモ。  
※セキュリティ的に、パスワードを平文でファイルには保存しない方針で。

#### ssh接続を鍵認証方式にする

BIG-IPの中身はRHELベースかと推測されますので、同じように設定可能。  
未検証ではあるが、設定のバックアップファイル(ucs)の中に、/homeとか/rootが含まれているようなので、
ハード故障・交換・ucsからのリカバリ時でも、鍵の再設定は不要と思われる。   
ただ、BIGのAnsibleモジュールは、REST APIのiControlをキックするようなので、
API用の認証ID・パスワードは別に考える必要がある。

#### ssh接続の認証を、コマンドラインオプションによって、毎回聞かれるようにする。

コマンドラインオプションで、ユーザ・パスワードを入力する。

```
# ansible-playbook xxx.yml -u testuser1 -k ・・・
SSH password:
```

鍵認証方式と同様、API用の認証ID・パスワードは別に考える必要がある。

#### iControlの認証用のパスワードを実行時に毎回聞かれるようにする。

Ansibleの[Promptsの機能](http://docs.ansible.com/ansible/playbooks_prompts.html)
を使用して、実行時に聞かれるようにする。  
以下は使用例：
```
- hosts: bigip
  gather_facts: no
  vars_prompt:
    - name: "lb_password"
      prompt: "Enter BIG-IP password"
      private: yes
  tasks:
    - name: bigip_command - Run commands on a BIG-IP via tmsh
      bigip_command:
        server: "{{ ansible_host }}"
        user: "testuser1"
        password: "{{ lb_password }}"
        command: "tmsh show sys version"
        validate_certs: "no"
      delegate_to: localhost
      register: result
    - debug: var=result.stdout_lines
```

```
ansible-playbook xxx.yml -u testuser1 -k　 ・・・
SSH password:
Enter BIG-IP password:
```

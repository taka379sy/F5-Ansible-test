# bigip_node - Manages F5 BIG-IP LTM nodes
BIG-IP上で、nodeの管理を行う。
## 検証環境
以下の環境で実施

- BIG-IP
  - BIG-IP 11.6.0 HF7（仮想版）
- ansible
  - CentOS 7.2.1511
  - ansible 2.2.0

## playbookの作成
以下の例では、nodeの名前がweb1をForced offlineに変更します。
```
# cd ~/work/f5-test
# vi bigip_node.yml
- hosts: bigip
  gather_facts: no
  tasks:
    - name: bigip_node - Manages F5 BIG-IP LTM nodes
      bigip_node:
        server: "{{ ansible_host }}"
        user: "{{ vault_remote_user }}"
        password: "{{ vault_password }}"
        name: "web1"
        session_state: "disabled"
        monitor_state: "disabled"
        validate_certs: "no"
      delegate_to: localhost
      register: result
    - debug: var=result
```
Enabledにするには、
```
monitor_state: enabled
session_state: enabled
```
Disabledにするには、
```
monitor_state: enabled
session_state: disabled
```
にすればよい。

## 実行
F5のgithub上のモジュール(~/work/f5-test/library/bigip_node.py)では、
```
(server, user, password,state, partition, validate_certs) = f5_parse_arguments(module)
ValueError: too many values to unpack
```
のようにエラーとなってしまう。  
ansible2.2.0に含まれるモジュール
(/usr/lib/python2.7/site-packages/ansible/modules/extras/network/f5/bigip_node.py
であればエラーにならなかったので、~/work/f5-test/library/bigip_node.pyは削除します。  
⇒ソース（bigip_node.py）を以下のように修正すれば動きました。
```
修正前）
    (server, user, password, state, partition, validate_certs ) = f5_parse_arguments(module)

修正後）
    (server, user, password, state, partition, validate_certs, server_port) = f5_parse_arguments(module)
```

コマンドの実行結果は、resultを出力すればよさそうです。
```
# ansible-playbook -i ~/work/f5-test/inventory/hosts -l bigip1 xxx.yml --ask-vault-pass
Vault password:

PLAY [bigip] *******************************************************************

TASK [bigip_node - Manages F5 BIG-IP LTM nodes] ********************************
changed: [bigip1 -> 127.0.0.1]

TASK [debug] *******************************************************************
ok: [bigip1] => {
    "result": {
        "changed": true
    }
}
```

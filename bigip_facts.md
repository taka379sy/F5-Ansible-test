# bigip_facts - Collect facts from F5 BIG-IP devices
情報収集
## 検証環境
以下の環境で実施

- BIG-IP
  - BIG-IP 11.6.0 HF7（仮想版）
- ansible
  - CentOS 7.2.1511
  - ansible 2.2.0

## playbookの作成
取得する情報の指定は、includeに","区切りで指定する。下記の例ではドキュメントに記載のある項目を全て指定してみた。
```
# cd ~/work/f5-test
# vi bigip_facts.yml
- hosts: bigip
  gather_facts: no
  tasks:
    - name: Collect facts from F5 BIG-IP devices
      bigip_facts:
        server: "{{ ansible_host }}"
        user: "{{ vault_remote_user }}"
        password: "{{ vault_password }}"
        include: address_class,certificate,client_ssl_profile,device,device_group,interface,key,node,pool,rule,self_ip,software,system_info,traffic_group,trunk,virtual_address,virtual_server,vlan
        validate_certs: "no"
      delegate_to: localhost
      register: ansible_facts
    - debug: var=ansible_facts
```
## 実行
コマンドの実行結果は、ansible_factsを出力すればよさそうです。
```
# ansible-playbook -i ~/work/f5-test/inventory/hosts -l bigip1 bigip_facts.yml --ask-vault-pass
Vault password:

PLAY [bigip] *******************************************************************

TASK [Collect facts from F5 BIG-IP devices] ************************************
ok: [bigip1 -> 127.0.0.1]

TASK [debug] *******************************************************************
ok: [bigip1] => {
    "ansible_facts": {
        "ansible_facts": {
            "address_class": {
                "/Common/aol": {
                    "address_class": [
<省略>

PLAY RECAP *********************************************************************
bigip1                     : ok=2    changed=0    unreachable=0    failed=0

```

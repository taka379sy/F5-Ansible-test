# bigip_irule - Manage iRules across different modules on a BIG-IP
iRuleの編集
## 検証環境
以下の環境で実施

- BIG-IP
  - BIG-IP 11.6.0 HF7（仮想版）
- ansible
  - CentOS 7.2.1511
  - ansible 2.2.0

## 編集するiRuleの作成
```
# cd ~/work/f5-test
# mkdir files
# vi files/irule-static.tcl
when LB_SELECTED {
   # Capture IP address chosen by WIP load balancing
   set wipHost [LB::server addr]
}

when LB_FAILED {
   set wipHost [LB::server addr]
}
```
## playbookの作成
```
# cd ~/work/f5-test
# vi bigip_irule.yml
- hosts: bigip
  gather_facts: no
  tasks:
    - name: Manage iRules across different modules on a BIG-IP
      bigip_irule:
        server: "{{ ansible_host }}"
        user: "{{ vault_remote_user }}"
        password: "{{ vault_password }}"
        module: ltm
        name: MyiRule
        src: files/irule-static.tcl
        state: present
        validate_certs: no
      delegate_to: localhost
      register: ansible_facts
    - debug: var=ansible_facts
```
## 実行
コマンドの実行結果は、ansible_factsを出力すればよさそうです。
```
# ansible-playbook -i ~/work/f5-test/inventory/hosts -l bigip1 bigip_irule.yml --ask-vault-pass
Vault password:

PLAY [bigip] *******************************************************************

TASK [Manage iRules across different modules on a BIG-IP] **********************
changed: [bigip1 -> 127.0.0.1]

TASK [debug] *******************************************************************
ok: [bigip1] => {
    "ansible_facts": {
        "api_anonymous": "when LB_SELECTED {\n   # Capture IP address chosen by WIP load balancing\n   set wipHost [LB::server addr]\n}\n\nwhen LB_FAILED {\n   set wipHost [LB::server addr]\n}\n",
        "changed": true,
        "name": "MyiRule",
        "partition": "Common"
    }
}

PLAY RECAP *********************************************************************
bigip1                     : ok=2    changed=1    unreachable=0    failed=0
```

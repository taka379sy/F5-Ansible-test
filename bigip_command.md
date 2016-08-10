# bigip_command - Run commands on a BIG-IP via tmsh
BIG-IP上で、tmshでコマンドを実行する。
## 検証環境
以下の環境で実施

- BIG-IP
  - BIG-IP 11.6.0 HF7（仮想版）
- ansible
  - CentOS 7.2.1511
  - ansible 2.2.0

## playbookの作成
以下の例では、"tmsh show sys version"を実行してみます。
```
# cd ~/work/f5-test
# vi bigip_command_test.yml
- hosts: bigip
  gather_facts: no
  tasks:
    - name: bigip_command - Run commands on a BIG-IP via tmsh
      bigip_command:
        server: "{{ ansible_host }}"
        user: "{{ vault_remote_user }}"
        password: "{{ vault_password }}"
        command: "tmsh show sys version"
        validate_certs: "no"
      delegate_to: localhost
      register: result
    - debug: var=result.stdout_lines
```
## 実行
コマンドの実行結果は、resultを出力すればよさそうです。
```
# ansible-playbook -i ~/work/f5-test/inventory/hosts -l bigip1 bigip_command_test.yml --ask-vault-pa
ss
Vault password:

PLAY [bigip] *******************************************************************

TASK [bigip_command - Run commands on a BIG-IP via tmsh] ***********************
changed: [bigip1 -> 127.0.0.1]

TASK [debug] *******************************************************************
ok: [bigip1] => {
    "result.stdout_lines": [
        "",
        "Sys::Version",
        "Main Package",
        "  Product     BIG-IP",
        "  Version     11.6.0",
        "  Build       7.0.446",
        "  Edition     Hotfix HF7",
        "  Date        Fri May 27 17:16:41 PDT 2016",
        "",
        "Hotfix List",
        "ID450033-5    ID516685-2   ID458928-5   ID479181     ID504880-2   ID509108-1",
  <省略>
        ""
    ]
}

PLAY RECAP *********************************************************************
bigip1                     : ok=2    changed=1    unreachable=0    failed=0
```

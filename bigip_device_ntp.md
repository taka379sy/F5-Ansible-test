# bigip_device_ntp - Manage NTP servers on a BIG-IP
NTPの設定
## 検証環境
以下の環境で実施

- BIG-IP
  - BIG-IP 11.6.0 HF7（仮想版）
- ansible
  - CentOS 7.2.1511
  - ansible 2.2.0

# playbookの作成
ドキュメントにある設定項目ntp_serverは、指定すると
```
unsupported parameter for module: ntp_server
```
と、エラーとなるので、ntp_serversを使用する。
```
# cd ~/work/f5-test
# vi bigip_device_ntp.yml
- hosts: all
  gather_facts: no
  tasks:
    - name: Manage NTP servers
      bigip_device_ntp:
        server: "{{ ansible_host }}"
        user: "{{ vault_remote_user }}"
        password: "{{ vault_password }}"
        ntp_servers:
          - "ntp.nict.jp"
        validate_certs: "no"
        timezone: "Japan"
      delegate_to: localhost
      register: result
    - debug: var=result.stdout_lines
```
## 実行
コマンドの実行結果は、resultを出力すればよさそうです。
```
# ansible-playbook -i ~/work/f5-test/inventory/hosts -l bigip1 bigip_device_ntp.yml --ask-vault-pass
Vault password:

PLAY [all] *********************************************************************

TASK [Manage NTP servers] ******************************************************
changed: [bigip1 -> 127.0.0.1]

TASK [debug] *******************************************************************
ok: [bigip1] => {
    "ansible_facts": {
        "changed": true,
        "ntp_servers": [
            "ntp.nict.jp"
        ],
        "timezone": "Japan"
    }
}

PLAY RECAP *********************************************************************
bigip1                     : ok=2    changed=1    unreachable=0    failed=0
```

# 初めに
BIG-IPをAnsibleで制御しようとしたところ、[Ansible公式ページ](http://docs.ansible.com/ansible/list_of_network_modules.html#f5)では、9つしかモジュールがありませんでした。(2016/8/5時点)  
これだけだと、やりたいことが出来ないと考えていたところ、
F5社が[github](https://github.com/F5Networks/f5-ansible)にAnsibleのモジュールを公開していることを知りました。  
モジュールの数もかなりあり、こちらであればやりたいことができそうでしたので、試してみることにしました。  

# 動作環境の設定
動作環境の設定は[こちら](./Setting_Up_the_Environment.md)

# f5-ansibleの設定
f5-ansibleの設定は[こちら](./Setting_Up_f5_ansible.md)

# モジュールの検証
## 検証環境
以下の環境で実施

- BIG-IP
  - BIG-IP 11.6.0 HF7（仮想版）
- ansible
  - CentOS 7.2.1511
  - ansible 2.2.0

## モジュール一覧-[リンク](https://f5-ansible.readthedocs.io/en/latest/modules/list_of_all_modules.html)  
以下のモジュールの検証をしていきます。全部を検証できる時間がないので一部のみになるかと思います。
説明が"foo"になっているところは、現時点でモジュールができていない、ドキュメントもできていないようです。
- [bigip_command - Run commands on a BIG-IP via tmsh](./bigip_command.md)  
BIG-IP上で、tmshでコマンドを実行する。
- [bigip_device_ntp - Manage NTP servers on a BIG-IP](./bigip_device_ntp.md)  
NTPの設定
- [bigip_facts - Collect facts from F5 BIG-IP devices](./bigip_facts.md)  
情報収集
- [bigip_irule - Manage iRules across different modules on a BIG-IP](./bigip_irule.md)  
iRuleの編集
- [bigip_node - Manages F5 BIG-IP LTM nodes](./bigip_node.md)  
nodeの管理を行う。

# 冗長構成で設定を同期させるには
冗長構成の場合、Device GroupsのAutomatic Syncにチェックを入れることで、変更が自動で同期されます。
こうすることで、１台の機器に対してAnsibleで変更した内容が、自動で他の機器に同期されます。

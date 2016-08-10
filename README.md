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
- [bigip_device_dns - Manage BIG-IP device DNS settings](./bigip_device_dns.md)
未検証
- [bigip_device_ntp - Manage NTP servers on a BIG-IP](./bigip_device_ntp.md)  
NTPの設定
- [bigip_device_sshd - Manage the SSHD settings of a BIG-IP](./bigip_device_sshd.md)
未検証
- [bigip_dns_record - Manage DNS resource records on a BIG-IP](./bigip_dns_record.md)
未検証
- [bigip_dns_record_facts - foo](./bigip_dns_record_facts.md)
未検証
- [bigip_dns_zone - Manages DNS zones on a BIG-IP](./bigip_dns_zone.md)
未検証
- [bigip_facts - Collect facts from F5 BIG-IP devices](./bigip_facts.md)  
情報収集
- [bigip_gtm_datacenter - Manage Datacenter configuration in BIG-IP](./bigip_gtm_datacenter.md)
未検証
- [bigip_gtm_facts - Collect facts from F5 BIG-IP GTM devices](./bigip_gtm_facts.md)
未検証
- [bigip_gtm_virtual_server - Manages F5 BIG-IP GTM virtual servers](./bigip_gtm_virtual_server.md)
未検証
- [bigip_gtm_wide_ip - Manages F5 BIG-IP GTM wide ip](./bigip_gtm_wide_ip.md)
未検証
- [bigip_hostname - Manage the hostname of a BIG-IP](./bigip_hostname.md)
未検証
- [bigip_iapp_service - foo](./bigip_iapp_service.md)
未検証
- [bigip_iapp_template - foo](./bigip_iapp_template.md)
未検証
- [bigip_irule - Manage iRules across different modules on a BIG-IP](./bigip_irule.md)  
iRuleの編集
- [bigip_license - Manage license installation and activation on BIG-IP devices](./bigip_license.md)
未検証
- [bigip_monitor_http - Manages F5 BIG-IP LTM http monitors](./bigip_monitor_http.md)
未検証
- [bigip_monitor_tcp - Manages F5 BIG-IP LTM tcp monitors](./bigip_monitor_tcp.md)
未検証
- [bigip_node - Manages F5 BIG-IP LTM nodes](./bigip_node.md)
未検証
- [bigip_partition - Manage BIG-IP partitions](./bigip_partition.md)
未検証
- [bigip_pool - Manages F5 BIG-IP LTM pools](./bigip_pool.md)
未検証
- [bigip_pool_member - Manages F5 BIG-IP LTM pool members](./bigip_pool_member.md)
未検証
- [bigip_provision - Manage BIG-IP module provisioning](./bigip_provision.md)
未検証
- [bigip_routedomain - Manage route domains on a BIG-IP](./bigip_routedomain.md)
未検証
- [bigip_routedomain_facts - Retrieve route domain attributes from a BIG-IP](./bigip_routedomain_facts.md)
未検証
- [bigip_selfip - Manage Self-IPs on a BIG-IP system](./bigip_selfip.md)
未検証
- [bigip_service - Manage BIG-IP service states](./bigip_service.md)
未検証
- [bigip_snmp - foo](./bigip_snmp.md)
未検証
- [bigip_software - Manage BIG-IP software versions and hotfixes](./bigip_software.md)
未検証
- [bigip_software_update - Manage the software update settings of a BIG-IP](./bigip_software_update.md)
未検証
- [bigip_sys_db - Manage BIG-IP system database variables](./bigip_sys_db.md)
未検証
- [bigip_sys_global - Manage BIG-IP global settings](./bigip_sys_global.md)
未検証
- [bigip_ucs - Manage UCS files](./bigip_ucs.md)
未検証
- [bigip_ucs_fetch - Fetches a UCS file from remote nodes](./bigip_ucs_fetch.md)
未検証
- [bigip_user - Manage user accounts and user attributes on a BIG-IP](./bigip_user.md)
未検証
- [bigip_user_facts - Retrieve user account attributes from a BIG-IP](./bigip_user_facts.md)
未検証
- [bigip_view - Manage ZoneRunner Views on a BIG-IP](./bigip_view.md)
未検証
- [bigip_virtual_server - Manages F5 BIG-IP LTM virtual servers](./bigip_virtual_server.md)
未検証
- [bigip_vlan - Manage VLANs on a BIG-IP system](./bigip_vlan.md)
未検証

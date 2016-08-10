# 動作環境の設定

## 検証環境
以下の環境で実施

- BIG-IP
  - BIG-IP 11.6.0 HF7（仮想版）
- ansible
  - CentOS 7.2.1511
  - ansible 2.2.0

### 動作環境
動作環境として、以下が要求されている。[リンク](https://github.com/F5Networks/f5-ansible#requirements)
- Ansible 2.2.0 or greater]installing
- Advanced shell for user account enabled
- bigsuds Python Client 1.0.4 or later
- f5-sdk Python Client, latest available

まずは、これらの環境を整える。
#### Ansible 2.2.0
Ansible公式ページに書かれている方式でインストール。[リンク](http://docs.ansible.com/ansible/intro_installation.html#latest-release-via-yum)

#### bigsuds Python Client 1.0.4
```
# pip install bigsuds
```

#### f5-sdk Python Client
```
# pip install f5-sdk
```

#### Advanced shell for user account enabled
ansibleで使用する、SSHログイン/iControlアクセスができるユーザを作成する。　　

1. BIG-IPの管理画面にアクセス
2. System > Users > Create を選択
3. 以下の設定でユーザ作成

```
User Name: testuser1 ※任意の文字列
Role: Administrator
Partition: ALL
Terminal Access: Advanced shell
```
※BIG-IP 11.6.1では、自分で作成したユーザは、iControlへのアクセスで認証（401）エラーになった。デフォルトで作成されているユーザadminであれば、Advanced shellの権限をつければアクセスできる。これ以外の回避策不明。

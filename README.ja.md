# sonatype-nexus3-ansible

Sonatype Nexus 3 (Repository Manager) をインストールするための Ansible playbook

参考 : https://help.sonatype.com/repomanager3/installation/system-requirements

> ※ これは過去に実際に使用していた Ansible playbook です。  
> 当時の環境を記録したものであり、現在の Nexus / CentOS / Ansible 環境で
> そのまま利用できることを保証するものではありません。

## 環境

- OS : CentOS8 ( KVM ゲスト )  
  ※ その後 Rocky9 にアップグレードして動作中

## 手順

CentOS8 に root でログインし以下の操作を行ってください。

※ CentOS8 は最小構成でインストールした想定です。

### Ansibleとgitのインストール

```
dnf install -y epel-release
dnf install -y ansible git
```

### playbook の転送

当プロジェクトをインストール先に転送してください。

### 設定変更

プレイブック内のファイル `group_vars/nexus_repository_manager` をエディタで開き、必要な設定変更を行ってください。

### playbook実行

転送した sonatype_nexus3_ansible ディレクトリに移動し、ansible-playbookコマンドを実行してください。  
インストールが開始されます。

```
cd sonatype_nexus3_ansible
ansible-playbook -i hosts site.yml
```

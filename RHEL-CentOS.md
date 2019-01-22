# 리포지터리/Yum Repository Reset

## 테스트

* RHEL 도커 이미지로 테스트 하면 됨.

```
docker run -it --rm registry.access.redhat.com/rhel7.6 bash
```
## RHEL을 CentOS로 사용하기
### RHEL subscription warning ignore

```
#subscription-manager 삭제
yum remove -y subscription-manager

# Set Disable
sed -i -e 's,\(^enabled\)=.*,\1=0,g' \
  /etc/yum/pluginconf.d/subscription-manager.conf
```


### 로컬 리포 생성 / Create Local repo on disk or file system
* 특정 디렉토리에 파일을 풀어 줌.

```
mkdir /tmp/mnt
mount -ro loop /share/CentOS/6.8/isos/x86_64/CentOS-6.8-x86_64-bin-DVD1.iso /tmp/mnt
rsync -avHPS /tmp/mnt/ /share/CentOS/6.8/os/x86_64/
umount /tmp/mnt
mount -ro loop /share/CentOS/6.8/isos/x86_64/CentOS-6.8-x86_64-bin-DVD2.iso /tmp/mnt
rsync -avHPS /tmp/mnt/ /share/CentOS/6.8/os/x86_64/
umount /tmp/mnt
```

### 리포 설정 파일 수정
* /etc/yum.repos.d/centos.repo
  * 리포 주소는 카카오쪽으로 설정
```

[base]
name=CentOS $releasever - Base/$basearch
baseurl=http://mirror.kakao.com/centos/7/os/$basearch/
gpgkey=http://mirror.kakao.com/centos/RPM-GPG-KEY-CentOS-7
enabled=1
priority=1
gpgcheck=0
[updates]
name=CentOS $releasever - Updates/$basearch
baseurl=http://mirror.kakao.com/centos/7/updates/$basearch/
gpgkey=http://mirror.kakao.com/centos/RPM-GPG-KEY-CentOS-7
enabled=1
priority=1
gpgcheck=0
[centosplus]
name=CentOS-$releasever - Plus
baseurl=http://mirror.kakao.com/centos/7/centosplus/$basearch/
gpgkey=http://mirror.kakao.com/centos/RPM-GPG-KEY-CentOS-7
exclude=kernel*
enabled=1
priority=1
gpgcheck=0
[extras]
name=CentOS $releasever - Extras/$basearch
baseurl=http://mirror.kakao.com/centos/7/extras/$basearch/
gpgkey=http://mirror.kakao.com/centos/RPM-GPG-KEY-CentOS-7
enabled=1
priority=1
gpgcheck=0
[epel7]
name=Fedora $releasever - EPEL7/$basearch
baseurl=http://mirror.kakao.com/epel/7/$basearch/
gpgkey=http://mirror.kakao.com/epel/RPM-GPG-KEY-EPEL-7
enabled=1
priority=3
gpgcheck=0
[epel7-server]
name=Fedora $releasever - EPEL7:Server/$basearch
baseurl=http://mirror.kakao.com/epel/7Server/$basearch/
gpgkey=http://mirror.kakao.com/epel/RPM-GPG-KEY-EPEL-7Server
enabled=1
priority=3
gpgcheck=0
```

### docker-ce추가

* original code from : https://download.docker.com/linux/centos/docker-ce.repo
  * ```wget -O /etc/yum.repos.d/docker-ce.repo https://download.docker.com/linux/centos/docker-ce.repo```

```
[docker-ce-stable]
name=Docker CE Stable - $basearch
baseurl=https://download.docker.com/linux/centos/7/$basearch/stable
enabled=1
gpgcheck=1
gpgkey=https://download.docker.com/linux/centos/gpg

[docker-ce-stable-debuginfo]
name=Docker CE Stable - Debuginfo $basearch
baseurl=https://download.docker.com/linux/centos/7/debug-$basearch/stable
enabled=0
gpgcheck=1
gpgkey=https://download.docker.com/linux/centos/gpg

[docker-ce-stable-source]
name=Docker CE Stable - Sources
baseurl=https://download.docker.com/linux/centos/7/source/stable
enabled=0
gpgcheck=1
gpgkey=https://download.docker.com/linux/centos/gpg

[docker-ce-edge]
name=Docker CE Edge - $basearch
baseurl=https://download.docker.com/linux/centos/7/$basearch/edge
enabled=0
gpgcheck=1
gpgkey=https://download.docker.com/linux/centos/gpg

[docker-ce-edge-debuginfo]
name=Docker CE Edge - Debuginfo $basearch
baseurl=https://download.docker.com/linux/centos/7/debug-$basearch/edge
enabled=0
gpgcheck=1
gpgkey=https://download.docker.com/linux/centos/gpg

[docker-ce-edge-source]
name=Docker CE Edge - Sources
baseurl=https://download.docker.com/linux/centos/7/source/edge
enabled=0
gpgcheck=1
gpgkey=https://download.docker.com/linux/centos/gpg

[docker-ce-test]
name=Docker CE Test - $basearch
baseurl=https://download.docker.com/linux/centos/7/$basearch/test
enabled=0
gpgcheck=1
gpgkey=https://download.docker.com/linux/centos/gpg

[docker-ce-test-debuginfo]
name=Docker CE Test - Debuginfo $basearch
baseurl=https://download.docker.com/linux/centos/7/debug-$basearch/test
enabled=0
gpgcheck=1
gpgkey=https://download.docker.com/linux/centos/gpg

[docker-ce-test-source]
name=Docker CE Test - Sources
baseurl=https://download.docker.com/linux/centos/7/source/test
enabled=0
gpgcheck=1
gpgkey=https://download.docker.com/linux/centos/gpg

[docker-ce-nightly]
name=Docker CE Nightly - $basearch
baseurl=https://download.docker.com/linux/centos/7/$basearch/nightly
enabled=0
gpgcheck=1
gpgkey=https://download.docker.com/linux/centos/gpg

[docker-ce-nightly-debuginfo]
name=Docker CE Nightly - Debuginfo $basearch
baseurl=https://download.docker.com/linux/centos/7/debug-$basearch/nightly
enabled=0
gpgcheck=1
gpgkey=https://download.docker.com/linux/centos/gpg

[docker-ce-nightly-source]
name=Docker CE Nightly - Sources
baseurl=https://download.docker.com/linux/centos/7/source/nightly
enabled=0
gpgcheck=1
gpgkey=https://download.docker.com/linux/centos/gpg

```



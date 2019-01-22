# 리포지터리/Yum Repository Reset

## 테스트

* RHEL 도커 이미지로 테스트 하면 됨.

```
docker run -it --rm registry.access.redhat.com/rhel7.6 bash
```

## 로컬 리포 생성 / Create Local repo on disk or file system
# 특정 디렉토리에 파일을 풀어 줌.
mkdir /tmp/mnt
mount -ro loop /share/CentOS/6.8/isos/x86_64/CentOS-6.8-x86_64-bin-DVD1.iso /tmp/mnt
rsync -avHPS /tmp/mnt/ /share/CentOS/6.8/os/x86_64/
umount /tmp/mnt
mount -ro loop /share/CentOS/6.8/isos/x86_64/CentOS-6.8-x86_64-bin-DVD2.iso /tmp/mnt
rsync -avHPS /tmp/mnt/ /share/CentOS/6.8/os/x86_64/
umount /tmp/mnt

## 리포 설정 파일 수정
* /etc/yum.repos.d/centos.repo
  * 리포 주소는 카카오쪽으로 설정
```
[base]
name=CentOS $releasever - Base/$basearch
baseurl=http://mirror.kakao.com/centos/$releasever/os/$basearch/
gpgkey=http://mirror.kakao.com/centos/RPM-GPG-KEY-CentOS-7
enabled=1
priority=1
gpgcheck=0
[updates]
name=CentOS $releasever - Updates/$basearch
baseurl=http://mirror.kakao.com/centos/$releasever/updates/$basearch/
gpgkey=http://mirror.kakao.com/centos/RPM-GPG-KEY-CentOS-7
enabled=1
priority=1
gpgcheck=0
[centosplus]
name=CentOS-$releasever - Plus
baseurl=http://mirror.kakao.com/centos/$releasever/centosplus/$basearch/
gpgkey=http://mirror.kakao.com/centos/RPM-GPG-KEY-CentOS-7
exclude=kernel*
enabled=1
priority=1
gpgcheck=0
[extras]
name=CentOS $releasever - Extras/$basearch
baseurl=http://mirror.kakao.com/centos/$releasever/extras/$basearch/
gpgkey=http://mirror.kakao.com/centos/RPM-GPG-KEY-CentOS-7
enabled=1
priority=1
gpgcheck=0
[epel7]
name=Fedora $releasever - EPEL7/$basearch
baseurl=http://mirror.kakao.com/epel/7/$basearch/
gpgkey=http://mirror.kakao.com/epel/RPM-GPG-KEY-EPEL-7
enabled=1
priority=2
gpgcheck=0
[epel7-server]
name=Fedora $releasever - EPEL7:Server/$basearch
baseurl=http://mirror.kakao.com/epel/7/$basearch/
gpgkey=http://mirror.kakao.com/epel/RPM-GPG-KEY-EPEL-7Server
enabled=1
priority=2
gpgcheck=0
```

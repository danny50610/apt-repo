# apt-repo

使用 [aptly](https://www.aptly.info/) 生成 Debian repository
來放一些自己編譯的套件

使用此 Repo 的使用方法請看：https://danny50610.github.io/apt-repo/

## aptly 使用筆記

### Init
在專案外面
```
aptly repo create -distribution=bionic -component=main apt-repo-bionic
aptly repo create -distribution=focal -component=main apt-repo-focal

aptly repo add apt-repo-bionic xxx.deb
aptly repo add apt-repo-focal xxx.deb

aptly snapshot create apt-repo-bionic from repo apt-repo-bionic
aptly snapshot create apt-repo-focal from repo apt-repo-focal

aptly publish snapshot -architectures=amd64 -component=main -gpg-key="7D85C61502D5634F" apt-repo-bionic filesystem:apt-repo:
aptly publish snapshot -architectures=amd64 -component=main -gpg-key="7D85C61502D5634F" apt-repo-focal filesystem:apt-repo:
```


### Add deb file
在專案外面 (記得要換 snapshot 名稱)
```
aptly repo add apt-repo-bionic xxx.deb
aptly repo add apt-repo-focal xxx.deb

aptly snapshot create apt-repo-bionic-2 from repo apt-repo-bionic
aptly snapshot create apt-repo-focal-2 from repo apt-repo-focal

aptly publish switch bionic filesystem:apt-repo: apt-repo-bionic-2
aptly publish switch focal filesystem:apt-repo: apt-repo-focal-2
```

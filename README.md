# apt-repo

使用 [aptly](https://www.aptly.info/) 生成 Debian repository  
來放一些自己編譯的套件

使用此 Repo 的使用方法請看：https://danny50610.github.io/apt-repo/

## aptly 使用筆記

### Init
#### Config
`.aptly.conf`
```json
...
"FileSystemPublishEndpoints": {
  "apt-repo": {
    "rootDir": "/home/root/apt-repo",
    "linkMethod": "copy",
    "verifyMethod": "md5"
  }
},
...
```

#### 建立 repo

在專案外面
```
aptly repo create -distribution=jammy -component=main apt-repo-jammy
aptly repo create -distribution=bionic -component=main apt-repo-bionic
aptly repo create -distribution=focal -component=main apt-repo-focal

aptly repo add apt-repo-jammy xxx.deb
aptly repo add apt-repo-bionic xxx.deb
aptly repo add apt-repo-focal xxx.deb

aptly snapshot create apt-repo-jammy from repo apt-repo-bionic
aptly snapshot create apt-repo-bionic from repo apt-repo-bionic
aptly snapshot create apt-repo-focal from repo apt-repo-focal

aptly publish snapshot -architectures=amd64 -component=main -gpg-key="7D85C61502D5634F" apt-repo-jammy filesystem:apt-repo:
aptly publish snapshot -architectures=amd64 -component=main -gpg-key="7D85C61502D5634F" apt-repo-bionic filesystem:apt-repo:
aptly publish snapshot -architectures=amd64 -component=main -gpg-key="7D85C61502D5634F" apt-repo-focal filesystem:apt-repo:
```


### Add deb file
在專案外面 (記得要換 snapshot 名稱)
```
aptly repo add apt-repo-jammy xxx.deb
aptly repo add apt-repo-bionic xxx.deb
aptly repo add apt-repo-focal xxx.deb

# snapshot 名稱不能重複，所以可能先用流水號
aptly snapshot create apt-repo-jammy-2 from repo apt-repo-bionic
aptly snapshot create apt-repo-bionic-2 from repo apt-repo-bionic
aptly snapshot create apt-repo-focal-2 from repo apt-repo-focal

aptly publish switch jammy filesystem:apt-repo: apt-repo-jammy-2
aptly publish switch bionic filesystem:apt-repo: apt-repo-bionic-2
aptly publish switch focal filesystem:apt-repo: apt-repo-focal-2
```

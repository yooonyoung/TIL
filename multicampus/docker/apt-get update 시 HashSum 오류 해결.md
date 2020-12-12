- sudo apt-get update 했을 때 HashSum 오류 발생

  ```
  Hit:1 http://archive.ubuntu.com/ubuntu bionic InRelease
  Get:2 http://security.ubuntu.com/ubuntu bionic-security InRelease [88.7 kB]
  Get:3 http://archive.ubuntu.com/ubuntu bionic-updates InRelease [88.7 kB]
  Get:4 http://security.ubuntu.com/ubuntu bionic-security/main amd64 Packages [868 kB]
  Get:5 http://archive.ubuntu.com/ubuntu bionic-backports InRelease [74.6 kB]
  Get:6 http://archive.ubuntu.com/ubuntu bionic/universe amd64 Packages [8570 kB]
  Get:7 http://security.ubuntu.com/ubuntu bionic-security/main Translation-en [269 kB]
  Get:8 http://security.ubuntu.com/ubuntu bionic-security/restricted amd64 Packages [95.5 kB]
  Get:9 http://security.ubuntu.com/ubuntu bionic-security/restricted Translation-en [20.4 kB]
  Get:10 http://security.ubuntu.com/ubuntu bionic-security/universe amd64 Packages [708 kB]
  Get:11 http://archive.ubuntu.com/ubuntu bionic/universe Translation-en [4941 kB]
  Get:12 http://security.ubuntu.com/ubuntu bionic-security/universe Translation-en [236 kB]
  Get:13 http://archive.ubuntu.com/ubuntu bionic/multiverse amd64 Packages [151 kB]
  Get:14 http://security.ubuntu.com/ubuntu bionic-security/multiverse amd64 Packages [8512 B]
  Get:15 http://security.ubuntu.com/ubuntu bionic-security/multiverse Translation-en [2908 B]
  Get:16 http://archive.ubuntu.com/ubuntu bionic/multiverse Translation-en [108 kB]
  Get:17 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 Packages [1089 kB]
  Err:17 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 Packages
    Hash Sum mismatch
    Hashes of expected file:
     - Filesize:1089328 [weak]
     - SHA256:8452acb0abd188816507c2d3aa4deeb508504cc6ff8ad1dc01ca347ff5d9ccd5
     - SHA1:5117d7a03db0fafe38c8057fe28ba9212f0c30d1 [weak]
     - MD5Sum:8b6bc4e734b05801f8921d4e1e7dec52 [weak]
    Hashes of received file:
     - SHA256:80963aef7e86a0307f1d7d11eb9e363288c21073839df5a31c3bf0ef6cdca99d
     - SHA1:45107a5a0b07601a49405341134575f4491c4d0d [weak]
     - MD5Sum:a1a818bd550a6200954d8d577269067a [weak]
     - Filesize:1089328 [weak]
    Last modification reported: Fri, 25 Sep 2020 12:57:58 +0000
    Release file created at: Fri, 25 Sep 2020 13:32:57 +0000
  Get:18 http://archive.ubuntu.com/ubuntu bionic-updates/main Translation-en [360 kB]
  Get:19 http://archive.ubuntu.com/ubuntu bionic-updates/restricted amd64 Packages [107 kB]
  Get:20 http://archive.ubuntu.com/ubuntu bionic-updates/restricted Translation-en [23.0 kB]
  Get:21 http://archive.ubuntu.com/ubuntu bionic-updates/universe amd64 Packages [1113 kB]
  Get:22 http://archive.ubuntu.com/ubuntu bionic-updates/universe Translation-en [348 kB]
  Get:23 http://archive.ubuntu.com/ubuntu bionic-updates/multiverse amd64 Packages [21.7 kB]
  Get:24 http://archive.ubuntu.com/ubuntu bionic-updates/multiverse Translation-en [6920 B]
  Get:25 http://archive.ubuntu.com/ubuntu bionic-backports/main amd64 Packages [7516 B]
  Get:26 http://archive.ubuntu.com/ubuntu bionic-backports/main Translation-en [4764 B]
  Get:27 http://archive.ubuntu.com/ubuntu bionic-backports/universe amd64 Packages [7736 B]
  Get:28 http://archive.ubuntu.com/ubuntu bionic-backports/universe Translation-en [4588 B]
  Fetched 19.3 MB in 19s (1035 kB/s)
  Reading package lists... Done
  E: Failed to fetch http://archive.ubuntu.com/ubuntu/dists/bionic-updates/main/binary-amd64/by-hash/SHA256/8452acb0abd188816507c2d3aa4deeb508504cc6ff8ad1dc01ca347ff5d9ccd5  Hash Sum mismatch
     Hashes of expected file:
      - Filesize:1089328 [weak]
      - SHA256:8452acb0abd188816507c2d3aa4deeb508504cc6ff8ad1dc01ca347ff5d9ccd5
      - SHA1:5117d7a03db0fafe38c8057fe28ba9212f0c30d1 [weak]
      - MD5Sum:8b6bc4e734b05801f8921d4e1e7dec52 [weak]
     Hashes of received file:
      - SHA256:80963aef7e86a0307f1d7d11eb9e363288c21073839df5a31c3bf0ef6cdca99d
      - SHA1:45107a5a0b07601a49405341134575f4491c4d0d [weak]
      - MD5Sum:a1a818bd550a6200954d8d577269067a [weak]
      - Filesize:1089328 [weak]
     Last modification reported: Fri, 25 Sep 2020 12:57:58 +0000
     Release file created at: Fri, 25 Sep 2020 13:32:57 +0000
  E: Some index files failed to download. They have been ignored, or old ones used instead.
  ```

  해결 방법 :

  ```
  $ sudo rm -rf /var/lib/apt/lists/*
  $ sudo apt-get update -o Acquire::CompressionTypes::Order::=gz
  $ sudo apt-get update && sudo apt-get upgrade
  ```

  
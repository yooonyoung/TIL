## 도커 Permission denied 에러나는 경우

### 상황

usermod로 사용자를 docker 그룹에 추가까지 했는데도 permission denied 발생

나의 경우는 Jenkins에서 `docker image build`를 자동화할 때 해당 에러가 발생했다.

```
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post 
```



### 해결

- `/var/run/docker.sock` 의 권한을 666으로 변경하여 그룹 내 다른 사용자도 접근 가능하도록 변경

  ```
  $ sudo chmod 666 /var/run/docker.sock
  ```

- 또는 `chown`으로 group ownership 변경

  ```
  $ sudo chown root:docker /var/run/docker.sock
  ```

- 처음부터는

  ```
  $ sudo /usr/sbin/groupadd -f docker
  $ sudo /usr/sbin/usermod -aG docker `user`
  $ sudo chown root:docker /var/run/docker.sock
  ```

  
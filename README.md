# Docker 명령어

```bash
$ docker run nginx
```

local상에서 image를 찾고 없으면 docker hub에서 image를 다운받는다.



```bash
$ docker ps -a
```

모든 container 보여줌. `-a` 를 붙히면 예전에 돌아간 container까지 모두 보여준다.



```bash
$ docker stop (container_name)
```

해당 container를 멈춤



```bash
$ docker rm (container_name)
```

해당 container를 삭제



```bash
$ docker images
```

모든 이미지 보여줌



```bash
$ docker rmi (image_name)
```

이미지 삭제 명령어 (해당 이미지에 의존하는 모든 컨테이너들을 멈추고 삭제해야 작동하는 명령어)



```bash
$ docker exec (container_name) cat /etc/...
```

docker container 안에서 명령어를 실행하고 싶을 때 사용하는 명령어 (해당 컨테이너가 돌아가고 있어야 함)



```bash
$ docker run -d (container_name)
```

해당 container를 background mode로 동작시키고 prompt로 돌아오는 명령어
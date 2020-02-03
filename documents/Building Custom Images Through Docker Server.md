### Creating Docker Images

지금까지 우리는 다른 개발자가 생성한 image를 docker hub에서 받아와 활용하는 것을 알아보았습니다. 지금부터는 실제 Dockerfile을 만들어 image와 container를 생성하고 실행하는 법을 배워보도록 하겠습니다.

Dockerfile에는 여기서 생성된 container가 어떻게 작동할지에 대한 구성이 들어가 있습니다. 이를 우리가 docker client를 통해 docker server로 전달하고 이를 전달받은 docker server가 사용할 수 있는 image를 생성합니다.

Dockerfile을 만드는데도 일정한 flow를 가집니다. 당장 용어가 생소하더라도 그냥 그렇다고 넘어가고 추후에 설명하도록 하겠습니다. 우선 base image를 정합니다. 그 다음 다른 필요한 프로그램들을 설치하기 위한 명령어를 작동시키고 마지막으로 container를 생성할 명령어를 정합니다.

| Specify a base image                                 |
| ---------------------------------------------------- |
| **Run some commands to install additional programs** |
| **Specify a command to run on container setup**      |

아래 코드가 Dockerfile의 예시입니다.

```dockerfile
# Use an existing docker image as a base
FROM alpine

# Download and install a dependency
RUN apk add --update redis

# Tell the image what to do when it starts
# as a container
CMD ["redis-server"]
```

해당 dockerfile을 빌드합니다.

```bash
$ docker build .
Sending build context to Docker daemon  2.048kB
Step 1/3 : FROM alpine
latest: Pulling from library/alpine
c9b1b535fdd9: Pull complete 
Digest: sha256:ab00606a42621fb68f2ed6ad3c88be54397f981a7b70a79db3d1172b11c4367d
Status: Downloaded newer image for alpine:latest
 ---> e7d92cdc71fe
Step 2/3 : RUN apk add --update redis
 ---> Running in 289c0b5028bc
fetch http://dl-cdn.alpinelinux.org/alpine/v3.11/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.11/community/x86_64/APKINDEX.tar.gz
(1/1) Installing redis (5.0.7-r0)
Executing redis-5.0.7-r0.pre-install
Executing redis-5.0.7-r0.post-install
Executing busybox-1.31.1-r9.trigger
OK: 7 MiB in 15 packages
Removing intermediate container 289c0b5028bc
 ---> 12c829d81145
Step 3/3 : CMD ["redis-server"]
 ---> Running in 5ab3ee1c4652
Removing intermediate container 5ab3ee1c4652
 ---> 6d55f8f84d10
Successfully built 6d55f8f84d10
```

마지막에 나오는 문자열을 복사해서 실행합니다.

```bash
$ docker run 6d55f8f84d10
1:C 03 Feb 2020 06:52:48.167 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 03 Feb 2020 06:52:48.167 # Redis version=5.0.7, bits=64, commit=bed89672, modified=0, pid=1, just started
1:C 03 Feb 2020 06:52:48.167 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
1:M 03 Feb 2020 06:52:48.195 * Running mode=standalone, port=6379.
1:M 03 Feb 2020 06:52:48.195 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
1:M 03 Feb 2020 06:52:48.195 # Server initialized
1:M 03 Feb 2020 06:52:48.195 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
1:M 03 Feb 2020 06:52:48.195 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
1:M 03 Feb 2020 06:52:48.195 * Ready to accept connections
```

자세한 내용은 다음 topic에서 설명하도록 하겠습니다.
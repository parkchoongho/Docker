# Manipulating Containers with the Docker Client

### Overriding Default Commands

```powershell
$ docker run hello-world
```

docker는 명령어가 이렇게 구성됩니다. `docker run [이미지 이름]` 순으로 구성되고 그 다음에는 명령어를 실어서 보낼 수 있습니다.

```powershell
$ docker run busybox echo hi there
hi there
```

이렇게 busybox에는 `echo hi there` 명령어를 같이 전달하면 결과가 콘솔창에 뜨는 것을 확인할 수 있습니다.

```powershell
$ docker run hello-world echo hi there
docker: Error response from daemon: OCI runtime create failed: container_linux.go:346: starting container process caused "exec: \"echo\": executable file not found in $PATH": unknown.
```

그런데 hello-world에서는 작동하지 않고 에러가 발생하는 것을 확인할 수 있습니다. 그 이유는 무엇일까요?

```powershell
$ docker run busybox ls
bin
dev
etc
home
proc
root
sys
tmp
usr
var
```

busybox image에는 여러개의 폴더들이 file system상 설치되어 있고 container가 만들어 질 때도 위 폴더들이 그대로 적용됩니다. 이 폴더 안에 해당 명령어를 읽을 수 있는 파일들이 들어있을테고 `ls, echo` 와 같은 명령어들을 실행할 수 있게 되는 것입니다. 하지만 hello-world image에는 정해진 text만을 보여주는 동작만 설정되어 있기 때문에 실행했을 때 해당 명령어를 읽어내지 못하고 에러가 발생한 것입니다.

```powershell
$ docker run hello-world ls
docker: Error response from daemon: OCI runtime create failed: container_linux.go:346: starting container process caused "exec: \"ls\": executable file not found in $PATH": unknown.
```

```powershell
$ docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

### Listing Running Containers

현재 돌아가고 있는 container를 보려면 

```powershell
$ docker ps
```

이와 같은 명령어를 입력하면 됩니다.

```powershell
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

하지만 아무런 container도 없는 것을 확인할 수 있습니다. 왜냐하면 지금까지 작성한 container는 모두 한번 동작하고 종료되는 container이었기 때문입니다. 따라서 어느 정도 시간동안 동작하는 명령을 내리도록 하겠습니다.

```powershell
$ docker run busybox ping google.com
```

위 명령어를 치고 다른 터미널을 열어 container를 확인해봅니다.

```powershell
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
211eca35b4e0        busybox             "ping google.com"   15 seconds ago      Up 12 seconds                           adoring_noyce
```

CONTAINER ID는 해당 container의 id를 의미합니다. IMAGE는 이 container가 어떤 image의 instance인지 알려주며 COMMAND는 어떤 명령어를 입력했는지 나타냅니다. CREATED는 언제 이 container가 생성됐는지, STATUS는 현재 상태, PORTS는 port, 번호 NAMES는 docker에서 부여하는 다른 container와 구분짓기 위한 고유한 이름입니다.

`docker ps --all` 명령어를 치면 지금까지 생성한 container를 모두 확인할 수 있습니다.

```powershell
$ docker ps --all
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
211eca35b4e0        busybox             "ping google.com"   4 minutes ago       Exited (0) 4 minutes ago                        adoring_noyce
2a234b85c6bb        hello-world         "/hello"            7 minutes ago       Exited (0) 7 minutes ago                        thirsty_faraday
9a5099360afc        hello-world         "ls"                8 minutes ago       Created                                         vibrant_goldstine
47d736b6557c        busybox             "ls"                11 minutes ago      Exited (0) 11 minutes ago                       tender_cerf
5af4e1eff1e9        hello-world         "echo hi there"     12 minutes ago      Created                                         wonderful_maxwell
484156be7870        busybox             "echo hi there"     16 minutes ago      Exited (0) 16 minutes ago                       charming_curran
c79f7e8f1ba6        busybox             "ls"                25 minutes ago      Exited (0) 25 minutes ago                       sleepy_blackwell
38952aff63b8        busybox             "ls"                26 minutes ago      Exited (0) 26 minutes ago                       inspiring_ishizaka
924da3db5792        busybox             "echo bye there"    27 minutes ago      Exited (0) 27 minutes ago                       trusting_ganguly
e83feb2b6f2c        busybox             "echo hi there"     27 minutes ago      Exited (0) 27 minutes ago                       hungry_burnell
a48a2a871eb3        hello-world         "/hello"            33 minutes ago      Exited (0) 33 minutes ago                       objective_meitner
c89d8d4bb866        hello-world         "/hello"            8 hours ago         Exited (0) 8 hours ago                          flamboyant_feynman
749a70e8c8c9        hello-world         "/hello"            9 hours ago         Exited (0) 9 hours ago                          kind_chaum
5d6bcb0bd0c4        hello-world         "/hello"            9 hours ago         Exited (0) 9 hours ago                          strange_cannon
9e378ea4b287        hello-world         "/hello"            9 hours ago         Exited (0) 9 hours ago                          hungry_ardinghelli
e7445dc569a4        hello-world         "/hello"            10 hours ago        Exited (0) 10 hours ago                         charming_neumann
dd3262f622cb        hello-world         "/hello"            10 hours ago        Exited (0) 10 hours ago                         infallible_swirles
```

### Container LifeCycle

`docker run`  `docker create` 명령어와 `docker start` 명령어를 모두 실행하는 명령어입니다. `docker create <image name>` `docker start <container id>` 이렇게 구성됩니다. create 명령어는 해당 image의 File System의 snapshot을 가져와 container에 setting하는 것까지를 의미합니다. start 명령어는 해당 startup command 또는 인자로 넘긴 command를 실행해 container를 돌리는 것을 의미합니다.

```powershell
$ docker create hello-world
e794f746892482b3190d40a55d2248a96fb85be4ae813e04da10a0e510f476c4
```

docker start 명령어는 start와 container id 사이에 -a를 입력해야 합니다.

```powershell
$ docker start -a e794f746892482b3190d40a55d2248a96fb85be4ae813e04da10a0e510f476c4

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

입력을 하지 않으면 아래와 같이 container id만 콘솔창에 찍혀서 나타납니다.

```powershell
$ docker start e794f746892482b3190d40a55d2248a96fb85be4ae813e04da10a0e510f476c4
e794f746892482b3190d40a55d2248a96fb85be4ae813e04da10a0e510f476c4
```

`-a` 명령어는 docker가 container 실행시, 실제 ouput이 보여지게끔 하는 역할을 합니다.

그리고 docker create 단계에서 생성된 override command는 후에 변경할 수 없습니다.

### Removing Stopped Containers

사용되지 않은 container를 삭제하는 법을 배워보겠습니다.

```bash
$ docker system prune
WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all dangling images
  - all dangling build cache

Are you sure you want to continue? [y/N] y
Deleted Containers:
04b6b26c2f6bef9bad216c86f90047b66c98736d99f788d6ae19b42452a91852
6e2c8a58baca24faa403e37eb7b5e79e955d2c3014fb94fef226ec449bfd9e8f
0c4651f33c1ce263f1a509ce68db0e7b8525bac2fe772ed38a21fed102c7a8a4
7422435a21244a08a759b681c804ff4c903e0d41d1e6c302a2ebbf1d022e398a
130073d5d1d9fbe361000811d68d12b3af7d17ac72d733756b30b3541d2ded8b
e794f746892482b3190d40a55d2248a96fb85be4ae813e04da10a0e510f476c4
211eca35b4e050a4b9b1d1d32642293d271c0726d66a11aa080e2f74bc3989e8
2a234b85c6bbf4f94a94f94e6982283e3358c479054384016938377a7fbee522
9a5099360afc187e79438f4b03a2af12a95f225b25a361b8cf7e4fcda0c8f085
47d736b6557c66e99694126525423eb291b400f8c94ada61ffbd53262550579e
5af4e1eff1e9de5eaa668b3ab5ac955a2218e2355b461a2e43c14a493524cde0
484156be787010885065dccec1298a43ef55c50a309a098e7e812c9badd9f68f
c79f7e8f1ba6de2a65a6fb84f6ea81225908411a5ec832bf77849deebc357d6b
38952aff63b8db8c8c25594c12f818bb8dbdd7ebde40585d91feb5e0465c75db
924da3db57920bde0e1ca062d61315e1086477430ac1177a98fe23bb4cc98daf
e83feb2b6f2c6d516db46fd9f31a24e17dc37144ae0abda0b74dfbed4ae2cf75
a48a2a871eb37309a4c48505bf367d25f75a24fb23830df4bb2c37924b2a262b
c89d8d4bb86634b243c0c88c3dad0607aad241422dfbaab772a5366eb477362f
749a70e8c8c9835d559f5ea9be70beff78ccb7087c21e59469bfd07d652a52fa
5d6bcb0bd0c4989ea16b22474a53694119a5ad400832f310149593baccb0c35d
9e378ea4b2872194e2d5b0ecdf41c42d14df7e8ef13d471a1989b697e8b85cf3
e7445dc569a4a12b26a99b76d8c16b7be9fee3d6d026dcc8f657fc106bebabe4
dd3262f622cb672553ff28111d144d398f956fdd4ade92624c190a7a9d254c36

Total reclaimed space: 0B
```

`docker system prune` 명령어를 치면 이렇게 멈춰있는 container들을 삭제합니다. (cache에 있는 image들도 삭제하기에 다음번에 `docker run hello-world` 명령어를 치면 docker hub로부터 image를 다운받게됩니다.)

```bash
$ docker ps --all
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

### Retrieving Log Outputs

컨테이너를 실행하는 작업이 매우 오래걸린다고 가정해봅시다. 실수로 `-a` 를 빼먹고 `docker start` 를 실행했다고 하면 다시 한번 컨테이너를 실행해야 할것입니다. 그러면 시간이 2배이상 들게됩니다.  이런경우 그전에 실행한 container의 결과가 어떤 것인지 그 log를 확인할 수 있으면 좋을 것 같습니다. 그것이 바로 `docker logs` 명령어입니다.

```bash
$ docker create busybox echo hi there
0f9515696d5e6599e9744c8cac6b30c01033279979e9788a9a6f4217a5ce058c
```

```bash
$ docker start 0f9515696d5e6599e9744c8cac6b30c01033279979e9788a9a6f4217a5ce058c
0f9515696d5e6599e9744c8cac6b30c01033279979e9788a9a6f4217a5ce058c
```

```bash
$ docker logs 0f9515696d5e6599e9744c8cac6b30c01033279979e9788a9a6f4217a5ce058c
hi there
```

### Stopping Containers

지금까지는 container들이 자동으로 종료되거나 실행되고 있는 프로세스를 Ctrl + C 를 통해 종료시켰습니다. 이번에는 docker 명령어를 통해 이 작업을 해보도록 하겠습니다.

```bash
$ docker create busybox ping google.com
1d55cb8341b965d8b516b8a875016bba28d17e07aceafd8df794bfc9f9372770
$ docker logs 1d55cb8341b965d8b516b8a875016bba28d17e07aceafd8df794bfc9f9372770
$ docker start 1d55cb8341b965d8b516b8a875016bba28d17e07aceafd8df794bfc9f9372770
1d55cb8341b965d8b516b8a875016bba28d17e07aceafd8df794bfc9f9372770
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
1d55cb8341b9        busybox             "ping google.com"   7 minutes ago       Up 16 seconds                           awesome_babbage
$ docker stop 1d55cb8341b9
1d55cb8341b9
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

`docker stop` 명령어를 치고 한동안 기다려야하는데 container에서 해당 명령어를 이해하지 못했기 때문입니다. docker stop은 container안에서 자체 종료되는 process로 유도하는 종료방법입니다. sigterm message를 전송해 container가 종료되도록 합니다. 하지만 ping명령어는 이를 이해하지 못하고 계속해서 돌고 싶어합니다. 이렇게 10초동안 container가 종료되지 않고 계속 돌아간다면 docker는 sigkill을 전송하고 강제로 container를 종료시켜버립니다. docker kill 명령어는 처음부터 sigkill을 container에 전송합니다.

```bash
$ docker start 1d55cb8341b9
1d55cb8341b9
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
1d55cb8341b9        busybox             "ping google.com"   9 minutes ago       Up 10 seconds                           awesome_babbage
$ docker kill 1d55cb8341b9  
1d55cb8341b9
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

### Multi-Command Containers

redis라는 image를 docker hub로 부터 받아와 실행해보도록 하겠습니다.

```bash
$ docker run redis
Unable to find image 'redis:latest' locally
latest: Pulling from library/redis
bc51dd8edc1b: Pull complete 
37d80eb324ee: Pull complete 
392b7748dfaf: Pull complete 
48df82c3534d: Pull complete 
2ec2bb0b4b0e: Pull complete 
1302bce0b2cb: Pull complete 
Digest: sha256:7b84b346c01e5a8d204a5bb30d4521bcc3a8535bbf90c660b8595fad248eae82
Status: Downloaded newer image for redis:latest
1:C 03 Feb 2020 04:25:52.593 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 03 Feb 2020 04:25:52.594 # Redis version=5.0.7, bits=64, commit=00000000, modified=0, pid=1, just started
1:C 03 Feb 2020 04:25:52.595 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
1:M 03 Feb 2020 04:25:52.602 * Running mode=standalone, port=6379.
1:M 03 Feb 2020 04:25:52.602 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
1:M 03 Feb 2020 04:25:52.602 # Server initialized
1:M 03 Feb 2020 04:25:52.602 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
1:M 03 Feb 2020 04:25:52.602 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
1:M 03 Feb 2020 04:25:52.602 * Ready to accept connections
```

그러면 이러한 명령어가 나타납니다. redis server가 돌고 있는 상태이므로 터미널을 하나 더 열어 redis-cli를 실행해보도록 하겠습니다.

```bash
$ redis-cli

Command 'redis-cli' not found, but can be installed with:

sudo apt install redis-tools
```

하지만 위 처럼 `redis-cli` 명령어를 찾지 못하게 됩니다. 왜냐하면 현재 redis server는 특정 container안에서 돌고 있으므로 만약 `redis-cli` 명령어를 실행하고 싶다면 그 명령어를 해당 container로 전달해주어야하기 때문입니다.

이런경우 사용하는 것이 `docekr exec -it <container id> <command>` 명령어입니다. `-it` 는 해당 container에 입력값을 넣을 권한을 부여하는 명령어입니다. 

```bash
$ docker exec -it 7220e89b7bbd redis-cli
127.0.0.1:6379> set myvalue 5
OK
127.0.0.1:6379> get myvalue
"5"
```

이렇게 해당 container안에서 redis-cli 를 실행할 수 있게 되었습니다.

`-it` 를 빼고 입력하면 redis-cli를 실행하지 못하고 다시 terminal 창으로 돌아오게됩니다.

```bash
$ docker exec 7220e89b7bbd redis-cli
$ 
```


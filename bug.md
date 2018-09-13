1. First comfirm docker-compose version and docker version
```
Bos-MacBook-Pro:test-docker-ignore byao$ docker-compose version
docker-compose version 1.18.0, build 8dd22a9
docker-py version: 2.6.1
CPython version: 2.7.12
OpenSSL version: OpenSSL 1.0.2j  26 Sep 2016

Bos-MacBook-Pro:test-docker-ignore byao$ docker version
Client:
 Version:	17.12.0-ce
 API version:	1.35
 Go version:	go1.9.2
 Git commit:	c97c6d6
 Built:	Wed Dec 27 20:03:51 2017
 OS/Arch:	darwin/amd64

Server:
 Engine:
  Version:	17.12.0-ce
  API version:	1.35 (minimum version 1.12)
  Go version:	go1.9.2
  Git commit:	c97c6d6
  Built:	Wed Dec 27 20:12:29 2017
  OS/Arch:	linux/amd64
  Experimental:	true
```

2. build via docker-compose
```
Bos-MacBook-Pro:test-docker-ignore byao$ docker-compose up --build
Building seth-rpc
Step 1/4 : FROM ubuntu:xenial
 ---> b9e15a5d1e1a
Step 2/4 : WORKDIR /project/sawtooth-seth/rpc
 ---> Using cache
 ---> f78fab8c934c
Step 3/4 : COPY rpc/ /project/sawtooth-seth/rpc
 ---> c3e5680033e2
Step 4/4 : RUN bash -c 'ls -al src/messages'
 ---> Running in 6f5d52841aa2
total 8
drwxr-xr-x 2 root root 4096 Sep 13 12:59 .
drwxr-xr-x 3 root root 4096 Sep 13 12:59 ..
Removing intermediate container 6f5d52841aa2
 ---> fed2f1e429ec
Successfully built fed2f1e429ec
Successfully tagged testdockerignore_seth-rpc:latest
Creating testdockerignore_seth-rpc_1 ... done
Attaching to testdockerignore_seth-rpc_1
testdockerignore_seth-rpc_1 exited with code 0
```

3. build the same dockerfile with docker
```
Bos-MacBook-Pro:test-docker-ignore byao$ docker build -f rpc/Dockerfile . --no-cache
Sending build context to Docker daemon  9.728kB
Step 1/4 : FROM ubuntu:xenial
 ---> b9e15a5d1e1a
Step 2/4 : WORKDIR /project/sawtooth-seth/rpc
 ---> Using cache
 ---> f78fab8c934c
Step 3/4 : COPY rpc/ /project/sawtooth-seth/rpc
 ---> ce2e49f66283
Step 4/4 : RUN bash -c 'ls -al src/messages'
 ---> Running in 3f6a057272f7
total 12
drwxr-xr-x 2 root root 4096 Sep 13 12:59 .
drwxr-xr-x 3 root root 4096 Sep 13 12:59 ..
-rw-r--r-- 1 root root    1 Sep 13 12:59 mod.rs
Removing intermediate container 3f6a057272f7
 ---> 9bac313d22c0
Successfully built 9bac313d22c0
```

4. Notice there's a `mod.rs` in step 3 but not in step 2, this caused sawtooth-seth-rpc build failed.

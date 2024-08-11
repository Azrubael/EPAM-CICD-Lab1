# 2024-08-11    12:39
=====================

**План работы**
1. Установка Docker Engine
2. Установка Minikube
3. Создание Dockerfile
4. Создание package.json
5. Создание server.js
6. Создание k8s-node-app.yaml
7. Создание workflow:
    ./.github/Workflows/deploy-to-minikube-github-actions.yaml
8. Создание коммита и проверка в GitHub actions


### 1 ### Установка Docker Engine
=================================
    $ sudo apt install cpu-checker

    $ kvm-ok
INFO: /dev/kvm exists
KVM acceleration can be used

    $ lsmod | grep kvm
kvm_intel             438272  0
kvm                  1138688  1 kvm_intel

# To check ownership of /dev/kvm:
    $ ls -al /dev/kvm
crw-rw----+ 1 root kvm 10, 232 Aug 11 10:01 /dev/kvm


# Install Docker Engine
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

# To install the latest version of Docker Engine
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# To verify installation
sudo docker run hello-world
    ...
Hello from Docker!
    ...


# Posinstall actions
    $ sudo groupadd docker
    $ sudo groupadd kvm

    $ whoami
    $ sudo usermod -aG kvm $USER
    $ sudo usermod -aG docker $USER


### LOG OUT
### LOG IN and check
    $ id
uid=1001(az) gid=1001(az) groups=1001(az),27(sudo),109(kvm),999(docker)
    $ docker run hello-world
    ...
Hello from Docker!
    ...



### 2 ### Установка Minikube
============================
# 2.1 ### https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fbinary+download
# 2.2 ### https://github.com/marketplace/actions/setup-minikube
# 2.3 ### https://github.com/medyagh/setup-minikube/tree/master/examples

# To install the latest minikube stable release on x86-64 Linux using binary download:

    $ curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 91.1M  100 91.1M    0     0   9.8M      0  0:00:09  0:00:09 --:--:-- 10.9M


   $ sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64

    $ minikube start
    ...
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default

# minikube can download the appropriate version of kubectl and you should be able to use it like this:
    $ minikube kubectl -- get po -A
    $ alias kubectl="minikube kubectl --"
    $ minikube pause        # Pause Kubernetes w/o impacting deployed apps
# OR
    $ minikube stop
✋  Stopping node "minikube"  ...
🛑  Powering off "minikube" via SSH ...
🛑  1 node stopped.

# Delete all of the minikube clusters:
    $ minikube delete --all


**To check if the container works well**
# Build the Docker image
```bash
docker build -t azw_app . && docker run --rm -d --name azw_container -p 3000:3000 azw_app
```
    ...
 => [internal] load build context                               0.1s
 => => transferring context: 263.24kB                           0.0s
 => [2/6] WORKDIR /usr/src/app                                  1.0s
 => [3/6] COPY package*.json ./                                 0.1s
 => [4/6] RUN npm install                                       4.3s
 => [5/6] RUN npm install express                               2.5s 
 => [6/6] COPY . .                                              0.1s 
 => exporting to image                                          0.3s 
 => => exporting layers                                         0.3s 
 => => writing image sha256:
fa6d68b8143f0b9a59472e7250b99f1205b2af7c68fe82e8ea93            0.0s 
 => => naming to docker.io/library/az_webapp                    0.0s
Running on http://0.0.0.0:3000


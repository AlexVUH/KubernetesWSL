### Ambiente de Testes
### Kubernetes / WSL Ubuntu Ubuntu-24.04 / Ubuntu 24.04 LTS
### Versão 0.1 - Documentação inicial
### 13/09/2025

```
PS C:\Users\avitola> wsl --install Ubuntu-24.04
Create a default Unix user account: avitola
```

```
sudo vim /etc/bash.bashrc
sudo vim ~/.bashrc
sudo vim /root/.bashrc

if [ "$EUID" -eq 0 ]; then
    # Prompt para root (vermelho)
    PS1='\[\e[01;31m\]\u@KubernetesWSL-LAB01\[\e[00m\]:\[\e[01;34m\]\w\[\e[00m\]\$ '
else
    # Prompt para usuário normal (verde)
    PS1='\[\e[01;32m\]\u@KubernetesWSL-LAB01\[\e[00m\]:\[\e[01;34m\]\w\[\e[00m\]\$ '
fi
```
#### Alterar a chave abaixo do registro (não é necessário usuário administrador)

```
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Lxss\
```

### Alterar o valor da chave "DistributionName" para "KubernetesWSL-LAB01"

### EXECUTAR TODOS OS PROCESSOS COMO ROOT

```
~$ sudo -s
```

### Instalando o Docker
```
~# apt update
~# apt upgrade
~# apt install net-tools
~# apt install docker.io
~# systemctl start docker
~# systemctl enable docker
~# apt install -y apt-transport-https ca-certificates curl
```

### Instalar o Certificado do Zscaler NECESSÁRIO PARA QUANDO ESTIVER NA VPN                             #
```
~# wget https://raw.githubusercontent.com/AlexVUH/WSL/refs/heads/main/ZscalerRootCertificate-2048-SHA256.crt

~# mv ZscalerRootCertificate-2048-SHA256.crt /etc/ssl/certs/
~# update-ca-certificates --fresh

~# curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor -o /usr/share/keyrings/kubernetes-apt-keyring.gpg

~# echo "deb [signed-by=/usr/share/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /" | tee /etc/apt/sources.list.d/kubernetes.list
```

### Para verificar a instalação, execute:
```
~# snap install core snapd
~# snap install kubectl --classic
~# kubectl version --client
```

### Instalando Minikube
```
~# curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
~# install minikube-linux-amd64 /usr/local/bin/minikube
~# minikube version
```
	## Se você encontrar um erro de privilégios de root, execute:
	```
	~# minikube delete --all 
	~# killall kubectl
	~# rm -rf /tmp/juju-mk*
	~# sysctl fs.protected_regular=0
	```
```
~# mkdir -p .minikube/certs
~# cd .minikube/certs/
~# wget https://raw.githubusercontent.com/AlexVUH/WSL/refs/heads/main/ZscalerRootCertificate-2048-SHA256.crt

~# minikube start --driver=docker --force
~# minikube status
~# kubectl cluster-info
~# kubectl config view
~# kubectl get nodes
~# kubectl get pods
~# minikube dashboard &
```

### Implantando um Aplicativo / Vamos implantar um simples servidor web Nginx usando kubectl:
```
~# kubectl create deployment my-nginx --image=nginx:latest
~# kubectl get deployment
~# kubectl get pods
```

### Expondo o Serviço
### Para expor a implantação do Nginx, podemos criar um serviço LoadBalancer:
```
~# kubectl expose deployment my-nginx --port=80 --type=LoadBalancer
```

### Agora você deve ver um endereço IP externo atribuído ao serviço Nginx
### que você pode usar para acessar o aplicativo implantado
```
~# kubectl get services
NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes   ClusterIP      10.96.0.1       <none>        443/TCP        10m
my-nginx     LoadBalancer   10.99.154.182   <pending>     80:30260/TCP   7m9s
```

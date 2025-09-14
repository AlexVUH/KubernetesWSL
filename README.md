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

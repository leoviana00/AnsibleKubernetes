# Levantar o ambiente

- Subir as vms
  vagrant up

  - k8s-master1 - IP: 192.168.0.2
  - k8s-worker1 - IP: 192.168.0.10
  - k8s-worker2 - IP: 192.168.0.11
  
- Configurar arquivo hosts (ansible) 

- Alterar o sshd_config - Permitindo o password authentication

  - $ sudo vi /etc/ssh/sshd_config
   PasswordAuthentication yes

- Reiniciar o serviço ssh

  - $ sudo systemctl restart sshd
---

# Instalação python 3
  - $ sudo yum install python3-pip -y
---

# Criar chave ssh

  - $ ssh-keygen -t rsa -C "your_email@example.com"
  - $ cat ~/.ssh/id_rsa.pub 
  --- 
- Criar usuário para compartilhamento das chaves ssh

  - $  sudo ssh-copy-id root@192.168.0.2
  - $  sudo ssh-copy-id root@192.168.0.10
  - $  sudo ssh-copy-id root@192.168.0.11
---

# Ansible  

- Teste de comunicação com módulo ping do ansible
  - $ ansible -i hosts -m ping all

    Criando roles:
  
  roles:
    - swap
    - usuarios
    - docker
    - kubernetes
    - master
    - worker

---

- $ ansible-galaxy init kubernetes (Role para instalação do kubernetes)
- $ ansible-galaxy init docker (Role para instalação do docker)
- $ ansible-galaxy init swap (Role para desabilitar o swap nas máquinas)
- $ ansible-galaxy init usuarios (Role para criação do usuário ansible)
- $ ansible-galaxy init master (Role para criação e configuração do cluster)
- $ ansible-galaxy init worker (Role para ingressar as máquinas ao cluster)

- $ ansible-playbook -i hosts playbook.yaml
  
---

# Comandos Kubernetes    

- https://kubernetes.io/pt/docs/reference/kubectl/cheatsheet/   ( comandos kubernetes )

---

# DASHBOARD

$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta4/aio/deploy/recommended.yaml

$ kubectl -n kubernetes-dashboard describe service kubernetes-dashboard


- DASHBOARD - USUÁRIO ADMIN (TOKEN)

$ kubectl apply -f kubernetes-dashboard-service-np.yaml 

$ kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')

--- 

#logon

https://192.168.0.2:30002/#/login (PODE SER IP DE QUALQUER UM DOS NÓS)

---

# DEPLOYMENT APP SPRING

1 - Criar o manifesto (app-spring.yaml)
       - Deploy
       - Container
       - Service 

2 - Criar o deployment 
  - $ kubectl apply -f app-spring.yaml  

3 - Listar os pods        
  - $ kubectl get pods -o wide

4 - Listar os services
  - $ kubectl get services

5 - Listar os deploys
  - $ kubectl get deployment

6 - Listar nodes
  - $ kubectl get nodes -o wide

7 - Informações do cluster
  - $ kubectl cluster-info

8 - Listar namespaces
  - $ kubectl get namespace

9 - Listar contextos
  - $ kubectl config get-contexts

Acessar o navegador ip:porta

---

# Nodes pegando IP INTERNO IGUAIS

- Arquivo 'fix-ip-nodes' com instruções para correção.

$ sudo touch /etc/sysconfig/network-scripts/ifcfg-eth0:1
$ sudo vi /etc/sysconfig/network-scripts/ifcfg-eth0:1

Copiar conteúdo abaixo para dentro do arquivo substituindo your.Float.ing.IP pelo seu IP. (CentOS)
___________________________________________________________________________

BOOTPROTO=static
DEVICE=eth0:1
IPADDR=your.Float.ing.IP
PREFIX=32
TYPE=Ethernet
USERCTL=no
ONBOOT=yes
___________________________________________________________________________

$ sudo systemctl restart network

- Referência: https://wiki.hetzner.de/index.php/Cloud_floating_IP_persistent/en

- Outra opção: (Testar)
  - nano /etc/networks/ (centos)
  - nano /etc/network/interfaces (ubuntu)

- $ sudo systemctl restart network (centos)
- $ /etc/init.d/networking restart (debian)

---

# - Restart kubelet
$ systemctl daemon-reload && systemctl restart kubelet



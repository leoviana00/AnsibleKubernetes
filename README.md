

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

- Criar usuário para compartilhamento das chaves ssh

  - $  sudo ssh-copy-id root@192.168.0.2
  - $  sudo ssh-copy-id root@192.168.0.10
  - $  sudo ssh-copy-id root@192.168.0.11
---

                            # Ansible  

- Teste de comunicação com módulo ping do ansible
  - $ ansible -i hosts -m ping all

- Role para desabilitar o swap nas máquinas
  - swap.yml

- Role para criação do usuário ansible
  - usuarios.yml

- Role para instalação das dependências
  - kube-deps.yml

- Role para criação e configuração do cluster
  - master.yml

- Role para ingressar as máquinas ao cluster
  - workers.yml

- Execução das playbooks:
  - $ ansible-playbook -i hosts nomeplaybook.yaml

__________________________________________________________________________
                               
                           COMANDOS KUBERNETES
__________________________________________________________________________    

# https://kubernetes.io/pt/docs/reference/kubectl/cheatsheet/   ( comandos kubernetes )                         

_________________________________________________________________________
                               
                               DASHBOARD
_________________________________________________________________________                               

$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta4/aio/deploy/recommended.yaml

$ kubectl -n kubernetes-dashboard describe service kubernetes-dashboard

_________________________________________________________________________
                         
                     DASHBOARD - USUÁRIO ADMIN (TOKEN)
_________________________________________________________________________


$ kubectl apply -f kubernetes-dashboard-service-np.yaml 

$ kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')  

#logon
https://192.168.0.2:30002/#/login (PODE SER IP DE QUALQUER UM DOS NÓS)


_________________________________________________________________________
                             
                              DEPLOYMENT APP SPRING
_________________________________________________________________________  
    
1 - Criar o manifesto (app-spring.yaml)
       - Deploy
       - Container
       - Service 

2 - Criar o deployment 
$ kubectl apply -f app-spring.yaml  
---

3 - Listar os pods        
$ kubectl get pods -o wide

4 - Listar os services
$ kubectl get services

5 - Listar os deploys
$ kubectl get deployment

6 - Listar nodes
$ kubectl get nodes -o wide

7 - Informações do cluster
$ kubectl cluster-info

8 - Listar namespaces
$ kubectl get namespace

9 - Listar contextos
$ kubectl config get-contexts

Acessar o navegador ip:porta


_________________________________________________________________________

                         Nodes pegando IP INTERNO IGUAIS
_________________________________________________________________________

         ################# ubuntu antes do 20.04 ##############
       
$ sudo touch /etc/network/interfaces.d/60-my-floating-ip.cfg
$ sudo nano /etc/network/interfaces.d/60-my-floating-ip.cfg 

---
auto eth0:1
iface eth0:1 inet static
    address your.Float.ing.IP
    netmask 32
---

$ sudo service networking restart


         ####################### ubuntu 20.04 ######################

$ sudo touch /etc/netplan/60-floating-ip.yaml
$ sudo nano /etc/netplan/60-floating-ip.yaml

---
network:
  version: 2
  ethernets:
    eth0:
      addresses:
      - your.float.ing.ip/32
---

$ sudo netplan apply

         ######################## centos ########################   

$ sudo touch /etc/sysconfig/network-scripts/ifcfg-eth0:1
$ sudo vi /etc/sysconfig/network-scripts/ifcfg-eth0:1

---
BOOTPROTO=static
DEVICE=eth0:1
IPADDR=your.Float.ing.IP
PREFIX=32
TYPE=Ethernet
USERCTL=no
ONBOOT=yes
---

$ sudo systemctl restart network

Referência: https://wiki.hetzner.de/index.php/Cloud_floating_IP_persistent/en

---
- Outra opção: 
nano /etc/network/ (centos)
nano  nano /etc/network/interfaces (ubuntu)
---
_________________________________________________________________________

                           REINICIAR KUBELET
_________________________________________________________________________

# - Restart kubelet
$ systemctl daemon-reload && systemctl restart kubelet



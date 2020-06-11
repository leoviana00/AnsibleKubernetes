

# Instalando HAProxy

- $ kubectl apply -f https://raw.githubusercontent.com/haproxytech/kubernetes-ingress/master/deploy/haproxy-ingress.yaml
---

# ESTRUTURA DO YAML:

- Um namespace colocar os recursos
- Uma ServiceAccount com a qual executar o Ingress Controller
- Uma ClusterRole para definir permissões
- Um ClusterRoleBinding para associar o ClusterRole à conta de serviço
- Um serviço padrão para lidar com todo o tráfego não roteado e retornar 404
- Uma ingress para implantar o pod do Ingress Controller
- Um service para permitir que o tráfego alcance o controlador de ingresso
---


# Portas mapeadas

Usando o comando abaixo poderemos ver as portas mapeadas
- $ kubectl get svc --namespace=haproxy-controller

- Container port 80 to NodePort 30279
- Container port 443 to NodePort 30775
- Container port 1024 to NodePort 31912

---

# Testando o envio de solicitação

Use curl para enviar uma solicitação com um cabeçalho Host do foo.bar e obtenha uma resposta 404:

- $ curl -I -H 'Host: foo.bar' 'http://192.168.0.10:30491'

---

# Adicionando um ingress


- Uma implantação para seus pods
- Um serviço para seus pods
- Um ingress que define o roteamento para seus pods
- Um ConfigMap para configurar o controlador

---

Arquivos:

- 1.ingress.yaml
- 2.configmap.yaml
---
Comandos:

- $ kubectl apply -f 1.ingress.yaml
- $ kubectl apply -f 2.configmap.yaml

---

# Testando novemete o envio de solicitação

- curl -I -H 'Host: foo.bar' 'http://192.168.0.10:30491'

O resultado tem que ser esse: HTTP/1.1 200 OK
---


# Deploy da aplicação desafio

- ingress-spring.yaml

- $ kubectl apply -f ingress-spring.yaml

- $ curl -s -XGET -H 'Host: desafio.spring' 'http://192.168.0.10:30491/desafio'

Resultado: Desafio01 | Spring Framework | Hello World




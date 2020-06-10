HAproxy, kubernetes e docker.

# Implantar o controlador de ingresso HAProxy Kubernetes

- Por padrão, o Ingress Controller assume que você deseja configurar o SSL. Se você preferir tentar coisas sem SSL, use o arquivo YAML e modifique o ConfigMap para que o redirecionamento de ssl esteja DESLIGADO.

kubectl apply -f haproxy-ingress.yaml

# Deploy da aplicação

- app.yaml 

Nota: 
A seção readinessProbe diz ao Kubernetes para enviar uma solicitação HTTP ao aplicativo cinco segundos após o início e, a cada cinco segundos, a partir de então. Nenhum tráfego é enviado para o pod até que uma resposta bem-sucedida seja retornada. Isso é essencial para evitar o tempo de inatividade.

- app-service.yaml 
Em seguida, defina um objeto Serviço que categorize os pods em um único grupo 

Em seguida, defina um objeto do Ingress. Isso configura como o HAProxy Ingress Controller encaminhará o tráfego para os pods
- ingress.yaml


Use o kubectl apply para implantar os pods, serviço e entrada:

--
- $ kubectl apply -f app.yaml -f app-service.yaml -f ingress.yaml
- $ kubectl get svc haproxy-ingress -n haproxy-controller
--


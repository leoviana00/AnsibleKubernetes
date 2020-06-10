# Deploy

- app-v1.yaml

Um objeto Deployment define uma seção spec.selector que corresponde à seção spec.template.metadata. É assim que uma implantação etiqueta os pods e os acompanha. Essa é a chave para configurar uma implantação verde azulada. Usando rótulos diferentes, você pode implantar várias versões do mesmo aplicativo. Aqui, a propriedade spec.selector.matchLabels está configurada para executar = app, versão = 0.0.1. A versão deve corresponder à tag de versão da sua imagem do Docker, por conveniência e simplicidade.

- app-service-bg.yaml

Em seguida, use a seguinte definição do Ingress para expor os pods da versão 1 ao mundo. Ele registra uma rota com o Controlador de ingresso HAProxy Kubernetes:

- ingress.yaml

# Aplicar usando Kubectl

- $ kubectl apply -f app-v1.yaml -f app-service-bg.yaml -f ingress.yaml
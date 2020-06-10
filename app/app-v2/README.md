# Atualizar usando uma implantação blue-green

Agora que a versão azul (ou seja, versão 1) foi lançada, crie uma versão verde do seu objeto Deployment que implantará a versão 2. A YAML será a mesma, exceto que você aumenta o valor do rótulo da versão e do Docker etiqueta de imagem. Observe também que o nome da implantação foi alterado de app-blue para app-green, pois você não pode ter duas implantações com o mesmo nome que segmentam pods diferentes.

- app-v2.yaml

Aplicar com kubectl

- $ kubectl apply -f app-v2.yaml


Nesse ponto, azul (versão 1) e verde (versão 2) são implantados. Apenas a instância azul está recebendo tráfego, no entanto. Para fazer a troca, atualize o seletor de versão da sua definição de serviço para que aponte para a nova versão:

-app-service.yaml

Aplicar com kubectl

- $ kubectl apply -f app-service.yaml


Verifique o aplicativo novamente e você verá que a nova versão está ativa. Se você precisar reverter para a versão anterior, basta alterar o seletor da definição de serviço e reaplicá-lo. O Controlador de ingresso HAProxy Kubernetes detecta essas alterações quase instantaneamente e você pode alternar entre o conteúdo do seu coração. Não há tempo de inatividade durante a transição. As conexões TCP estabelecidas terminarão normalmente na instância em que começaram.

Testando os novos pods:

Você também pode testar a nova versão antes do lançamento registrando uma rota de entrada diferente que expõe o aplicativo em um novo caminho de URL. Primeiro, crie outra definição de serviço chamada serviço de teste:

- test-service.yaml

Em seguida, adicione uma nova rota ao seu objeto Ingress existente que exponha esse serviço no caminho / teste da URL, conforme mostrado:

- ingress.yaml

Isso permite que você verifique seu aplicativo visitando / testando no seu navegador. Observe que atualmente seu aplicativo de back-end precisará reescrever a URL para que / test seja substituído por /, pois a reescrita de caminho ainda não é suportada no HAProxy Kubernetes Ingress Controller. O aplicativo de amostra lida com isso.
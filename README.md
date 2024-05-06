# Servidor Web Nginx com Balanceador de Carga e Auto Scaling usando Kubernetes

Este repositório contém um guia passo a passo para criar um servidor web nginx com balanceador de carga e auto scaling usando Kubernetes, juntamente com instruções para realizar testes de carga usando Vegeta.

## Passo 1: Configurar o Cluster Kubernetes

1. Instale o Kubernetes: Siga as instruções de instalação do Minikube para o seu sistema operacional disponíveis em [Minikube Docs](https://minikube.sigs.k8s.io/docs/start/).
   
2. Inicie o Cluster: Execute o comando `minikube start` para iniciar o cluster Kubernetes.

## Passo 2: Criar o Deployment do nginx

O Deployment é uma abstração no Kubernetes que gerencia um conjunto de réplicas de um aplicativo. No arquivo nginx-deployment.yaml, definimos o Deployment do nginx com 3 réplicas. Isso significa que o Kubernetes garantirá que sempre haja três instâncias do nginx em execução.

1. Crie o arquivo `nginx-deployment.yaml` com o conteúdo fornecido no guia.
   
2. Aplique o Deployment usando o comando `kubectl apply -f nginx-deployment.yaml`.

## Passo 3: Criar o Serviço do nginx

O Serviço é uma abstração no Kubernetes que define uma política de acesso para as Pods (instâncias) do seu aplicativo. No arquivo nginx-service.yaml, definimos um Serviço do tipo LoadBalancer que expõe o nginx para o tráfego externo na porta 80.

1. Crie o arquivo `nginx-service.yaml` com o conteúdo fornecido no guia.
   
2. Aplique o Serviço usando o comando `kubectl apply -f nginx-service.yaml`.

## Passo 4: Configurar o Auto Scaling

O HPA (Horizontal Pod Autoscaler) é uma característica do Kubernetes que ajusta automaticamente o número de réplicas de um Deployment com base na utilização da CPU. No arquivo nginx-hpa.yaml, definimos um HPA para o Deployment do nginx que dimensionará o número de réplicas entre 2 e 5, mantendo a utilização da CPU em cada réplica em 50%.

1. Crie o arquivo `nginx-hpa.yaml` com o conteúdo fornecido no guia.
   
2. Aplique o HPA usando o comando `kubectl apply -f nginx-hpa.yaml`.

## Passo 5: Testar o Balanceamento de Carga

1. Obtenha o endereço IP do Serviço usando o comando `minikube service nginx-service --url`.
   
2. Acesse o nginx pelo navegador ou usando cURL para verificar o balanceamento de carga.

## Passo 6: Aplicar Testes de Carga usando Vegeta

O Vegeta é uma ferramenta de teste de carga de linha de comando escrita em Go. É simples de usar e permite enviar um grande volume de solicitações HTTP concorrentes.

1. Certifique-se de que o Vegeta está instalado na sua máquina. Você pode instalar usando Go ou baixando o binário diretamente.

2. Crie um arquivo chamado nginx-targets.txt que contenha o endpoint do serviço nginx que você deseja testar.

3. Use o comando vegeta attack -targets=nginx-targets.txt -rate=50 -duration=30s | vegeta report para executar o teste de carga. Isso enviará uma carga de 50 requisições por segundo para o serviço nginx durante 30 segundos.


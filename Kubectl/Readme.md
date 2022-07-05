# Comandos kubectl
## Comandos em geral:
- kubectl api-resources
    - Lista todos os recursos disponíveis no Kubernetes, geralmente utilizado para sabermos qual APIVERSION deveremos utilizar no yaml.
## Comando de informações:

- Caso trabalhe com namespaces temos 2 formas de ober as informações so comando que estamos utilizando:
    - Utilizando o nome do namespace dirretamente -n NAME_NAMESPACE, ex:
        - kubectl logs NAME_POD -n NAME_NAMESPACE
    - Ou podemos passar o --all-namenspaces, para listar tudo de todos os namespaces, ex:
        -  kubectl logs NAME_POD --all-namenspaces
- kubectl get <objeto>
    - Obs: <objeto> é o nome do recurso que queremos verificar
    - nodes
        - Podemos verificar os nodes que estão no cluster
    - pods
        - Podemos verificar os pods/containers que estão rodando no cluster
        - Flags:
            - -l: Lista os pods que estão com o label informado.
                - Exemplo: kubectl get pods -l versao=verde
            - -o wide: Lista os pods com informações mais detalhadas
        - Explicação das colunas:
            - NAME: Nome do pod
            - READY: Quantos containers estão rodando e quantos containers eu tenho dentro do pod
            - STATUS: Status do pod
            - RESTARTS: Quantas vezes o pod foi reiniciado
            - AGE: Tempo que o pod está rodando
    - replicaset
        - Podemos verificar os replicasets que estão rodando no cluster.
    - deployments
        - Podemos verificar os deployments que estão rodando no cluster e seus estados.
        - Flags:
            - --all-namespaces: Lista os deployments em todos os namespaces
    - services
        - Podemos verificar os services que estão rodando no cluster seus Types, Ips, etc.
    - all
        - Podemos verificar todos os recursos que estão rodando no cluster kubernetes.
    - endpoints
        - Podemos verificar os endpoints que estão rodando no cluster, e os IPS dos pods que estão vinculado a este endpoint.
    - namespaces
        - Podemos verificar os namespaces que estão no cluster.
- kubectl describe <objeto>
    - pod <nome_do_pod>
        - Podemos verificar mais informações sobre o pod, se nao colocar o <nome_do_pod> ele mostra as informações de todos os pods
    - replicaset <nome_do_replicaset>
        - Exibe mais informações sobre o replicaset
- kubectl logs <nome_do_pod>
    - Podemos verificar o log do pod
- kubectl top pod <nome_do_pod>
    - Utilizado para verificar a quantidade de recurso que um pod está utilizando, se rodar o mesmo e der o erro "Metrics API not available", é preciso intalar o Metric Server:
````
        kubectl apply -f https://raw.githubusercontent.com/pythianarora/total-practice/master/sample-kubernetes-code/metrics-server.yaml
````
## Comandos de ações:
- kubectl apply -f <arquivo_de_configuracao>
    - Para subir os arquivos yaml para rodar nos pods.
    - Caso quiser subir mais de um arquivo, basta colocar o -f <arquivo_de_configuracao> -f <arquivo_de_configuracao>
    - -n <namespace>
        - Para subir os arquivos para o namespace informado.
        - Exemplo: kubectl apply -f <arquivo_de_configuracao> -n <namespace>
    - -R
        - Realiza um apply recursivo ou seja todos os yamls que tiver na pasta ele vai executar.
- kubectl rollout <objeto>
    - history deployment <nome_do_deployment>
        - Para verificar o histórico de alterações do deployment
    - undo deployment <nome_do_deployment>
        - Para desfazer as alterações do deployment, voltar a versão.
- kubectl delete <obejeto>
    pod <nome_do_pod>
        - Podemos deletar um pod em especifico
    -f <arquivo_de_configuracao>
        - Podemos deletar os pods de arquivo de configuração
    - Flags:
        - -l: Deleta os pods que estão com o label informado
            - Exemplo: kubectl delete pod -l versao=verde
        - -f: Deleta os pods que estão no arquivo informado
            - Exemplo: kubectl delete pod -f <arquivo_de_configuracao>
- kubectl port-forward <nome_do_pod> <porta_local>:<porta_do_pod>
    - Realizar o bind de uma porta de um POD para outra porta, para acessar localmente.

- kubectl scale deployment <nome_do_deployment> --replicas=<quantidade_de_pods>
    - Podemos alterar a quantidade de pods que estão rodando no cluster.
    - Exemplo: kubectl scale deployment <nome_do_deployment> --replicas=2

- kubectl create namespace <nome_do_namespace>
    - Podemos criar um namespace no cluster.
    - Exemplo: kubectl create namespace <nome_do_namespace>

- kubectl create configmap literal-configmap --from-literal=Mongo__Host=mongo-service
- kubectl create configmap file-configmap --from-file=NAME_FILE.yaml
- kubectl create secret generic literal-secret --from-literal=MONGO_PWD=mongopwd
- kubectl create secret generic file-secret --from-file=NAME_FILE.txt

- kubectl create <objeto>
    - configmap
        - Utilizado para criar Config Maps por linha de comando literalmentepara isso podemos fazer de 2 formas, Chave=Valor ou com arquivos.
        - literal-configmap
            - Criando a partir de Chave=Valor
            - Flags:
                - --from-literal=<chave>=<valor>
                    - Criando por linha de comando.
        - file-configmap
            - Criando a partir de um arquivo
            - Flags:
                - --from-file=<nome_do_arquivo_com_extensão>
                    - Criando a partir de um arquivo o configmap.
    - secret
        - Utilizado para criar Secrets por linha de comando, para isso podemos fazer de 2 formas, chave=valor ou com arquivos.
        - literal-secret
            - Criando a partir de Chave=Valor
            - Flags:
                - --from-literal=<chave>=<valor>
                    - Criando por linha de comando.
        - file-secret
            - Criando a partir de um arquivo
            - Flags:
                - --from-file=<nome_do_arquivo_com_extensão>
                    - Criando a partir de um arquivo o configmap.

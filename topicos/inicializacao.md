


### Inicializando

Para inicializar o cluster precisamos do kubeadm, e o primeiro passo é baixar todas as imagens necessarias para sua utilização no master.
 os passos a seguir seram feitos somente no master.

```
$ kubeadm config images pull
```  
Nesse passo vamos baixar tudo que é necessario para inicialização do cluster.  

Agora vamos inicializar o master

```
$ kubeadm init
```  

Apos fazer esses passo, o k8s deve ter sido inicializado normalmente.  


[IMG]  


Agora vamos fazer a configuraçao do k8s para que possa ser usado ocm o meu usuario, como ele mostra na imagem acima.  

```  
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```  

Apos realizar os passo falado no k8s, crie um podnetwork, para que pods em diferentes nodes consigam se comunicar.

```
$ kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
```
mais informaçoes https://kubernetes.io/docs/concepts/cluster-administration/addons/#networking-and-network-policy  



### listando os pods  


Bom, agora podemos veriaficar informaçoes sobre os nossos nodes e nossos podemos.

Verificar nodes  
```
$ kubectl get pods --all-namespaces  
```

Verificar pods  
```  
$ kubectl get pods  
```  
Verificar pods de todos os namespaces  
```  
$ kubectl get pods --all-namespaces  
```  
Verificar de um determinado namespace  
```
$ kubectl get pods -n kube-system  
```   

### Add Maquinas no cluster

Agora vamos adicionar as maquinas no nosso cluster, para isso basta copiar a linha do token informado no master, nas demais maquinas. Caso voce nao consiga achar esse linha bastas digitar o comando a seguir.  

```
$ kubeadm token create --print-join-command
```  
Esse comando mostra a linha que voce precisa para adicionar os nos. Agora basta copiar a linha de saida e executar nos nodes que voce quer adicionar.  

```
$ kubeadm join <ip> --token <hash do token>
```  

Resetar o cluster(executar em cada nó)  
kubectl reset --force (se nao for o master nao precisa do force)

Informaçoes sobre o node
kubectl describ node <nome>  


namespace

kubectl get namespaces  


Parametros  

Kubectl get pods <parametros>  

-n <nome_namespace>
  mostra os pods do namespaces informado.  
-o wide
  mostra mais informaçoes sobre os pods  



kubectl describd pods  

-o yaml|json  
  traz todso os detalhas do deployment

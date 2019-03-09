# Instalação  

Primeiro vamos criar um arquivo de configuraçao para que o kernel possa inicializar os modulos quando der o boot do sistema. Essa configuração precisa ser feita em todas as maquinas que iram fazer parte do nosso cluster.  

Vamos la:  

*vi /etc/modules-load.d/k8s.conf*  
```
br_netfilter
ip_vs_rr
ip_vs_wrr
ip_vs_sh
nf_conntrack_ipv4
ip_vs
```

Como nao vou reiniciar a maqui na primeira vez, vamos usar o *modprobe* para inicializar eles.  

```
$ modprobe br_netfilter ip_vs_rr ip_vs_wrr ip_vs_sh nf_conntrack_ipv4 ip_vs  
```

Após fazer a inicializaçao dos modulos, vamos fazer a atualização do sistema.

```  
$ apt-get update && apt-get upgrade
```  

### Docker  

Agora precisamos instalar o Docker nas nossas maquinas, para isso digite:

```  
$ curl -fsSl https://get.docker.com | bash   
```

Assim a última versão do docker estara instalada nas maquinas.

Mais informaçoes sobre Docker, [clique aqui](https://docs.docker.com/).  

### Confirguração dos repositorios  

Nesse momento vamos adicionar os repositórios do K8s no nossos nodes. Para isso vamos editar nosso  **source list**.

Para isso basta seguir esses passos:
```  
$ apt-get update && apt-get install -y apt-transport-https
$ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -  
$ echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" > /etc/apt/sources.list.d/kubernetes.list
$ apt-get update
$ apt-get install -y kubelet kubeadm kubectl
```

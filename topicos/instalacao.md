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

Para os modulos serem carregados é preciso reiniciar a maquina. Aqui vamos usar o *modprobe* para inicializar eles sem precisar reiniciar as maquinas.  

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
$ docker --version
```

Assim a última versão do docker estara instalada nas maquinas.

Mais informaçoes sobre Docker, [clique aqui](https://docs.docker.com/).  
Primeiros passo com Docker, [clique aqui](https://adrianocanofre.dev/tutorial/docker-basico/).  


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

Neste momento fizemos inserimos o repositório do k8s nos nossos nodes, e fizemos a instalação do packages.  

Agora vamos verificar se o driver cgroup usado pelo kubelete é o mesmo usado pelo docker. Para verificar isso, basta seguir os passo:  

```  
$ docker info | grep -i cgroup  
  Cgroup Driver: cgroupfs  
```
Como podemos ver nesse caso, o driver utilizado pelo docker é o **cgroupfs**, e é esse valor q utilizaremos no cgroup do k8s.  

```  
$ sed -i "s/cgroup-driver=systemd/cgroup-driver=cgroupfs/g" /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
$ systemctl daemon-reload
$ systemctl restart kubelet
```  


Mais informaçoes https://kubernetes.io/docs/setup/independent/install-kubeadm/.


Caso esteja utilizando VMs é preciso desligar a swap. Para finalizar a Instalação precisamos desabilitar nossa swap.

```  
$ swapoff -a  
```  

Comente tambem a referencia no fstab.

```  
$ vi /etc/fstab  
```

Nosso proximo passo é fazer a inicialização do nosso cluster.

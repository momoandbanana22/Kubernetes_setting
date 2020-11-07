# Kubernetes_setting

https://qiita.com/soumi/items/7736ac3aabbbe4fb474a

# [M/W] curl
* install
    ```
    $ sudo apt install -y curl
    ```

# [M/W] docker
*  install
    ```
    $ sudo apt-get install -y docker.io
    ```
# [M/W] kubernetes
* 今回はkubeadmを使用してインストールするので、まずはkubeadmをインストールする必要があります。  
* デフォルトのリポジトリにkubeadmが無いので、まずkubernetes のキーとリポジトリを登録  
    ```
    $ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
    $ sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
    ```
* その後、kubeadmをインストールします。
    ```
    $ sudo apt update
    $ sudo apt install kubeadm
    ```
# [M] Kubernetesをセットアップ
いよいよkubernetesをインストールします！
…といってもkubeadmを使うので、数手でインストール完了してしまいます。

flannel 用にオプション付きで init します。

```
$ sudo kubeadm init --pod-network-cidr=192.168.1.0/24
```
```
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.1.10:6443 --token ezi8g0.z5us0732x4n71mre \
    --discovery-token-ca-cert-hash sha256:e1e4559655b7367921ceb6a04b9928b59ca9c2cdffa4269e02240c7846ed9bf6
```
```
$ mkdir -p $HOME/.kube
$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
$ sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
これでKubernetesから言われたことは全て実行したので、一度ノードの様子を見てみます。
```
$ kubectl get nodes
```
```
momoandbanana@tpX200s-lubuntu:~$ kubectl get nodes
NAME              STATUS     ROLES    AGE     VERSION
tpx200s-lubuntu   NotReady   master   3m19s   v1.19.3
momoandbanana@tpX200s-lubuntu:~$ 
```
# [M] flannelのためにiptablesを編集
これをしないと、後で作られるcore-dnsというPodが永遠にCrashLoopBackOff状態になって起動しません。
(ここでハマりました…)
```
sudo sysctl net.bridge.bridge-nf-call-iptables=1
```
# [M] flannel
```
$ kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```
```
podsecuritypolicy.policy/psp.flannel.unprivileged created
clusterrole.rbac.authorization.k8s.io/flannel created
clusterrolebinding.rbac.authorization.k8s.io/flannel created
serviceaccount/flannel created
configmap/kube-flannel-cfg created
daemonset.apps/kube-flannel-ds created
```

# jenkins-KubernetesCluster-Creation

![kub](https://github.com/VardhanLearn/Jenkins-KubernetesCluster/assets/87961252/7b927f46-49fd-4a7d-96d1-5ff88a7289c1)


Before you need to setup Jenkins & docker, kubernetes 1.0 plugins to deploy this project

Step1:
On Master & worker node

sudo su
apt-get update
apt-get install docker.io -y

service docker restart
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" >/etc/apt/sources.list.d/kubernetes.list
apt-get update
apt install kubeadm=1.20.0-00 kubectl=1.20.0-00 kubelet=1.20.0-00 -y

Step2:
On Master:

kubeadm init --pod-network-cidr=192.168.0.0/16
->Copy the token and paste it into the worker node.

Step3:
On Master

 exit
     mkdir -p $HOME/.kube
     sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
     sudo chown $(id -u):$(id -g) $HOME/.kube/config

Step4
On Master

 kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
 kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-
 v0.49.0/deploy/static/provider/baremetal/deploy.yaml

Step5
On Worker node

apt-get update -> add the token in worker node
kubeadm join 172.31.39.91:6443 --token sk0cdf.7xji41c450cu1u59 \
    --discovery-token-ca-cert-hash sha256:653d5de59aa2825a3cd3903a0877274530e0f75571b19eca313c76ee21047354

Check the "Kubectl get nodes" command in master-controlpane -> to check Workernode connected to master or not.

Step6

Add below jenkins stage to pipeline & run the build

	stage('Deploy to K8s') {
          steps {
            script {
                
                kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'kubeconfig')
            }
          }
        }

Finally build success.



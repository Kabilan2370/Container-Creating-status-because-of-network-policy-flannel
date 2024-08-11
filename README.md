# Container-Creating-status-because-of-network-policy-flannel
unable to create a container on kubernets, To solve this problem we should try different network polices

![Alt text](/error/3.png)

![Alt text](/error/9.png)

Flannel runs a small, single binary agent called flanneld on each host, and is responsible for allocating a subnet lease to each host out of a larger,
preconfigured address space. Flannel uses either the Kubernetes API or etcd directly to store the network configuration, the allocated subnets,
and any auxiliary data (such as the host's public IP). Packets are forwarded using one of several backend mechanisms including VXLAN and various cloud integrations.

[ Problems ] : While create pods an my k8s master node , it was being a container creation status. To describe the pod ,
               the peoblem was flannel network policy does not work properly on machine.

1. Let's, find out the problem and try to solve. Using init commant initialize your on master node.

        kubeadm init --ignore-preflight-errors=all
or

        kubeadm init --pod-network-cidr=30.320.0.0/16  --ignore-preflight-errors=NumCPU  --ignore-preflight-errors=Mem

2. When we to start using cluster , we should run several instruction on master machine.

       kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml

![Alt text](/error/1.png)   

3. Here , I took three ec2 machines. Already installed every packages on every machine.

   ![Alt text](/error/2.png)

4. After , I apply a flannel network policy on master machine. Everynodes always has good condition.

   ![Alt text](/error/2.png)

5. For test , Create a pod as mypod1 and exposed also used following command.

   ![Alt text](/error/3.png)

6. Check my pod status How thats are running, I could not complete a pod creation.
   I also tried couple of times, i got the same pending creation status.

   ![Alt text](/error/4.png)

7. To resolve this problem, use these following calico.yaml link
   How do I install a pod network add-on like Calico after initializing the control plane?

       kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml

   ![Alt text](/error/5.png)

8. Now again create and expose a another pod....

       kubectl run second-test --image=bnvschaitanya/maven-web-application --port=78

   To expose...

       kubectl expose pod second-test --type=NodePort --port=78 --target-port=8081

  ![Alt text](/error/7.png)

9. Now check get pod to view, is everythink working fine or not ? Yes , it working fine.

    ![Alt text](/error/8.png)

   

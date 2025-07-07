## Steps to Deploy in Kubernetes Cluster:

Enable DNS resolution on kubernetes cluster :
> Check coredns pod in kube-system namespace
```bash
kubectl get pods -n kube-system -o wide | grep -i core
```
> Scale coredns pod on worker node as well for DNS resolution
```bash
kubectl edit deploy coredns -n kube-system -o yaml
```
Edit replica from 2 to 4
![image](https://github.com/user-attachments/assets/f1f4f026-bcfe-42cc-8361-890035e5973e)

![image](https://github.com/user-attachments/assets/5c680291-6497-404a-ba23-115e391df7bb)

> Deploy manifest files to kubernets cluster
```
kubectl create -f kubernetes/
```
> Get all objects inside the namespace
<img width="1422" alt="Screenshot 2025-07-06 at 10 59 35 PM" src="https://github.com/user-attachments/assets/960cd204-6ade-4565-b61e-6eb234e4177b" />

> See the logs in case of deployment errors
```bash
kubectl describe pod <pod-name> 
kubectl logs <pod-name>
```
> Now test the application
```bash
kubectl port-forward service/frontend 31000:5173 --address 0.0.0.0 &
```

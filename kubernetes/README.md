## Steps to Deploy in Kubernetes Cluster:

### Prerequisite:
* Update the **.env.docker** file in **frontend** directory
```bash
VITE_API_PATH="http://192.168.68.88:31100" # this ip from worker node
```
* Update the **.env.docker** file in **backend** directory
```bash
MONGODB_URI="mongodb://mongo-service/wanderlust"
```
* Rebuild the docker image with tag and push it to DockerHub
```bash
cd fronetnd
docker build -t <your-dockuerhub-username>/frontend:07.07.25 .
cd backend
docker build -t <your-dockuerhub-username>/backend:07.07.25 .
docker login -u <your-dockuerhub-username>
docker images
docker push <your-dockuerhub-username>/frontend:<TAG>
docker push <your-dockuerhub-username>/backend:<TAG>
```
<img width="1422" alt="Screenshot 2025-07-06 at 11 38 18 PM" src="https://github.com/user-attachments/assets/115e2b00-74c0-4be8-95d0-df7c4f8a9aab" />

<img width="1422" alt="Screenshot 2025-07-06 at 11 39 55 PM" src="https://github.com/user-attachments/assets/73269175-08bc-443c-8792-a78ad4cc8f83" />

<img width="1440" alt="Screenshot 2025-07-06 at 11 40 49 PM" src="https://github.com/user-attachments/assets/3e4d0985-1db0-4b8f-bfdf-b65241b2d374" />

* Now once the image is pushed you can just update the image tag in frontend-deployment.yaml which you pushed above.
<img width="807" alt="Screenshot 2025-07-06 at 11 44 36 PM" src="https://github.com/user-attachments/assets/3432d46c-5bf2-41d5-8148-b1e27a5fa8b6" />

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
> Now before adding data, first check and make sure you are in project root
```bash
pwd
```
<img width="1422" alt="Screenshot 2025-07-06 at 11 02 33 PM" src="https://github.com/user-attachments/assets/870cdaa0-0288-4d10-b59c-040159981066" />

> Add data to mongo db wanderlust DB for that copy file to mongo pod
```bash
kubectl cp ./backend/data/sample_posts.json mongo-deployment-56d49bdcf6–48g59:/sample_posts.json
```
> Connect to mongo pod
```bash
kubectl exec -it <mongo-deployment-7-pod-name> -- /bin/bash
```
> Execute mongoimport command
```bash
mongoimport --db wanderlust --collection posts --file sample_posts.json --jsonArray
```
Results:
> Get the PublicIP and connect frontend:
```bash
kubectl port-forward service/frontend 31000:5173 --address 0.0.0.0 &
```

![image](https://github.com/user-attachments/assets/451ef39f-0ed7-48e8-84cd-d90c4e82ac2e)

> Go inside mongodb pod:
```bash
kubectl exec -it <mongo-deployment-pod> -- /bin/bash
```
<img width="1422" alt="Screenshot 2025-07-06 at 11 16 47 PM" src="https://github.com/user-attachments/assets/85f27add-3388-498c-80c7-36fc292a3115" />

> We can check sample data login in the DB shell
```bash
mongo
show dbs;
use wanderlust;
show tables;
show collections;
```

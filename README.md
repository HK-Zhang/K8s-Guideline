## Welcome Kubernetes Learning Group

You can use the [editor on GitHub](https://github.com/HK-Zhang/K8s-Guideline/edit/gh-pages/README.md) to maintain and preview the content in Markdown files.

## 1. Setup a single node cluster at local
There are two solutions to setup a single node cluster on Windows 10. Docker for windows(Edge) is the easiest and recommended way to get Kubernetes at local. You have to make sure your pc's internet connection is setup correctely (has access on www.google.com is a must).
* [Setup with Docker for windows edge](https://www.hanselman.com/blog/HowToSetUpKubernetesOnWindows10WithDockerForWindowsAndRunASPNETCore.aspx)
* [Setup with Minikube](https://medium.com/@JockDaRock/minikube-on-windows-10-with-hyper-v-6ef0f4dc158c)

### 1.1 Post installation check

To check whether k8s is created sucessfully on you local, You could run command `kubectl config get-contexts`

Below is going to show on the terminal if you setup k8s with Minikube.

![minikube context](/images/minikube.JPG)

Below is going to show on the terminal if you setup k8s with Docker for windows. Context might be missing, to fix it you have to add `C:\Users\[replace with username]\.kube\config` to `HOME` environment variable.

![docker context](/images/dockerkube.jpg)

### 1.2 Enable Dashboard
Run command: `kubectl proxy`

Output should be as below:

![docker proxy](/images/dockerproxy.JPG)

Then openlink: http://localhost:8001/api/v1/namespaces/kube-system/services/http:kubernetes-dashboard:/proxy/#!/cluster?namespaces=_all&namespace=default

![dashboard](/images/dashboard.JPG)

## 2. Prepare a docker image
There is a simple node app docker file. build and push it to docker repository as  below. Please replace 'yxzhk' as your own signature registered on docker hub. source code and configuration file is located [here](https://github.com/HK-Zhang/k8s-Guideline/tree/gh-pages/service)

`docker build -t yxzhk/nodeapp .`

`docker push yxzhk/nodeapp`

## 3. K8s deployment
Run command: `kubectl apply -f kubia-deploy.yaml`

![deploy](/images/deploymentrun.JPG)

Run: `kubectl get pods`

![getpods](/images/getpodsrun.JPG)

Run: `kubectl get deployments`

![getdeploy](/images/getdeploymentrun.JPG)

Check dashboard:

![getpods](/images/dbpod.JPG)

![getpods](/images/dbdeployment.JPG)

once K8s finish the deployment, check status again.

![poddeployment](/images/poddep.JPG)

![poddeployment](/images/dbpod1.JPG)

![poddeployment](/images/dbdepl1.JPG)

## 4. K8s service

### 4.1 Create Service
Run command: `kubectl apply -f kubia-svc.yaml`

Check results:

![service](/images/service.JPG)

![service](/images/deservice.JPG)

### 4.2 Access Service

Access service from a pod as bleow command format: Kubectl exec {pod name} -- curl -s http://{service ip} 

In our case, Run: `Kubectl exec kubia-deployment-8497d7fdd9-hrccm -- curl -s http://10.105.16.224
`
![accessservice](/images/accessservice.JPG)

Discovering service through environment variables: kubectl exec {pod name} env

In our case, Run: `Kubectl exec kubia-deployment-8497d7fdd9-hrccm env`

![envservice](/images/serviceenv.JPG)

### 4.3 Access Service through FDQN

Connecting to the service through its FDQN(fully qualified domain name)
servicename.default.svc.cluster.local

Run Command: `kubectl exec -it kubia-deployment-8497d7fdd9-hrccm bash`

Them Run: `curl http://kubia.default.svc.cluster.local` or

`curl http://kubia.default` or

`curl http://kubia`

![bash in pod](/images/bashinpod.JPG)

### 4.4 Service Endpoints

![service endpoints](/images/serviceendpoint.JPG)

## 5. K8s network
Source code can be found [here](https://github.com/HK-Zhang/k8s-Guideline/tree/gh-pages/Network)

Run Command: `kubectl apply -f deployment.yaml`

<span style='background-color:gray;'>deployment "service-test" created</span>

run: `kubectl get pods`

run: `kubectl get pods --selector=app=service_test_pod -o jsonpath='{.items[*].status.podIP}'`

update podclient.html with one of above IP address.


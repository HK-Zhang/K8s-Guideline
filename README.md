## Welcome Kubernetes Learning Group

You can use the [editor on GitHub](https://github.com/HK-Zhang/K8s-Guideline/edit/gh-pages/README.md) to maintain and preview the content in Markdown files.

## Setup a single node cluster at local
There are two solutions to setup a single node cluster on Windows 10. Docker for windows(Edge) is the easiest and recommended way to get Kubernetes at local. You have to make sure your pc's internet connection is setup correctely (has access on www.google.com is a must).
* [Setup with Docker for windows edge](https://www.hanselman.com/blog/HowToSetUpKubernetesOnWindows10WithDockerForWindowsAndRunASPNETCore.aspx)
* [Setup with Minikube](https://medium.com/@JockDaRock/minikube-on-windows-10-with-hyper-v-6ef0f4dc158c)

### Post installation check

To check whether k8s is created sucessfully on you local, You could run command `kubectl config get-contexts`

Below is going to show on the terminal if you setup k8s with Minikube.
![minikube context](/images/minikube.JPG)

Below is going to show on the terminal if you setup k8s with Docker for windows. Context might be missing, to fix it you have to add C:\Users\[replace with username]\.kube\config to HOME environment variable.

![docker context](/images/dockerkube.jpg)

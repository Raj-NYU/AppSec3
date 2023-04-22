# Monitoring with Prometheus Part 3

## Part 3.1 - Removing unwanted monitoring
After reading the article provided, I analyzed the view.py code to ensure that there were no sensitive information exposures. Upon inspection, The use of graphs[pword].inc() in the view.py code incremented a Prometheus metric with a password value. However, this password value was also displayed in the logs during a POST request, which could potentially expose sensitive information to unauthorized individuals.. To remediate this issue, I commented out a portion of the code (line 51 - 57). Additionally, I commented out the tracker for the number of login requests (line 17 - 30) to prevent revealing a user's interactions.

Helpful article:
https://danuka-praneeth.medium.com/setting-up-a-comprehensive-monitoring-system-with-prometheus-6b2b73cf54d2


## Part 3.2 - Removing reasonable monitoring
For this part, I analyzed the view.py file and noticed that the '404 not found' message was being used in multiple parts of the code. I decided to track the number of times this message is returned in the code, so I added the line graphs['error_return_counter'].inc() beneath each line of code where '404 not found' was returned.
I identified four specific locations in the code where this message was being returned:

lines 107-109

lines 114-116

lines 161-163

lines 168-170

By adding the graphs['error_return_counter'].inc() line in each of these locations, I can effectively track the number of times the '404 not found' message is returned in the code. 
Tracking 404 not found errors can be useful for security reasons because it can help identify potential security vulnerabilities in the code.
When a user requests a resource that does not exist on the server, the server returns a 404 not found response. However, if an attacker is probing the server for sensitive information or trying to exploit a vulnerability, they may intentionally request resources that do not exist in order to elicit a 404 not found response. By tracking the frequency of 404 not found errors, developers can identify whether there is an unusual level of such requests, which may indicate an attack or attempted exploit.


## Part 3.3 - Adding Prometheus
I started this part by downloading helm by running the commands:
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```
I then installed prometheus by using helm by running the commands:
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install prometheus-community/prometheus --generate-name
```
Then to ensure prometheus was running properly I ran the following commands:
```
kubectl get services
kubectl get pods
```
Then in order to see all the running services and to ensure prometheus service was up I ran the command:
```
minikube service --all
```
Then in order to find the proper prometheus server to its port I ran the following command:
```
kubectl expose service prometheus-19862113000-server --type=NodePort --target-port=9090 --name=prometheus-server-np
```
Doing this opened up a webpage from the running prometheus server. Now I had to play around with the configmap so that the server would be able to read/use our data from our website. In order to do this I ran the following commands:
```
kubectl get configmap
kubectl edit configmap prometheus-19862113000-server
```
I then added the following code for the server to monitor the giftcardsite below (line 25 - 29) within the prometheus-server.yaml:
```
- job_name: GiftCardSite_monitoring
static_configs:
- targets:
- proxy-service:8080
```
I then copied the configfile which I made edits to a yaml file called prometheus-server.yaml by running the commands:
```
kubectl get configmap prometheus-19862113000-server -o yaml > prometheus-server.yaml
```
After I checked to make sure that the server was pointing to a port (80) and was running properly by running the command:
```
minikube service list prometheus
kubectl delete <pod>
kubectl get pods
```
Some articles that I used were:
https://github.com/prometheus-community/helm-charts
https://prometheus.io/docs/introduction/first_steps/


# Securing Secrets Part 1

To enhance security, I started by reviewing the settings.py file and discovered that it contained hardcoded secrets. This poses a security risk, so I decided to remove the hardcoded secret key and store it in an environment variable in a separate secretkey.yaml file.
Upon examining the provided files, I discovered that both the django-deploy.yaml and dp-deployment.yaml files contained hardcoded secrets as well, which is also a security hazard. To address this, I created a secretkey.yaml file using the same format as the django-admin-pass-secret.yaml file, which would store all sensitive information and secrets in base64 format by running the command:
```
echo <key> | base64.
```
Once the secretkey.yaml file was created to store the sensitive information/secrets, I applied it by running the command: 
```
kubectl apply -f secretkey.yaml
```
I confirmed that the secretkey.yaml file was successfully applied by running the command:
```
kubectl get secrets
```
I then updated the django-deploy.yaml and dp-deployment.yaml files to implement the changes by removing any hardcoded secrets and replacing them with (secretKeyRef) the secrets from the secretkey.yaml file.
To ensure that the changes were applied, I ran the command:
```
kubectl delete pods <pod name> 
```
Then to confirm that the plaintext secrets had been replaced with the encrypted secrets from the secretkey.yaml file I ran the command:
```
kubectl exec -it <pod name> /bin/sh
```
--

***Check submission on gradescope for screenshots of commands and files within my repository for additional information***

# Applying Migrations Part 2

## Migration Job
For this part we need to run migrations so I decided to create migrationjob.yaml file that would hold all the variables that would be carried over and implemented the commands python3 manage.py migrate within the file to make sure this would trigger the django migration job by the manage.py file. 
After the yaml file is created I needed to apply it by running the command:
```
kubectl apply -f migrationjob.yaml
```
Then to confirm that the yaml file has been implemented and completed I ran the command:
```
kubectl get job
kubectl get pods
```

## Seeding Job
For this part we need to populate the records from one table to another. I started to create a seedingjob.yaml file that would make a call to the database with the stored credentials within the yaml flie. Within this file I implemented an arguement that would make a call through a separate sql with the args parameter. 
I also updated the Docker file by commenting out the data folder and the setup.sql command since its been ran already.
In order to implement and confirm for completion I ran the commands:
```
kubectl apply -f seedingjob.yaml
kubectl get job
kubectl get pods
```

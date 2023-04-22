# Applying Migrations Part 2

## Migration Job
To run the migrations, I have created a migrationjob.yaml file that contains all the necessary variables. I have added the commands to run the migrations in the file using python3 manage.py migrate. This ensures that the Django migration job is triggered by the manage.py file.

To apply the migrationjob.yaml file, I ran the command:
```
kubectl apply -f migrationjob.yaml
```

Then to confirm that the yaml file has been implemented and completed I ran the command:
```
kubectl get job
kubectl get pods
```

## Seeding Job
To populate the records from one table to another, I have created a seedingjob.yaml file that will make a call to the database using the stored credentials within the yaml file. I have also implemented an argument in the file that will make a call through a separate SQL with the args parameter.
To implement and confirm completion, I have updated the Docker file by commenting out the data folder and the setup.sql command since it has already been executed.

To apply the seedingjob.yaml file, I ran the command:
```
kubectl apply -f seedingjob.yaml
```

Then to confirm that the yaml file has been implemented and completed I ran the commands:
```
kubectl get job
kubectl get pods
```
***Check submission on gradescope for screenshots of commands and files within my repository for additional information***

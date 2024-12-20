Project 1.
Setup
1. Clone source from git
2. Run list of these commands to setup
    Create EKS with script
        eksctl create cluster --name my-cluster --region us-east-1 --nodegroup-name my-nodes --node-type t3.small --nodes 1 --nodes-min 1 --nodes-max 2

    Config cluster with local kubectl CI
        aws eks --region us-east-1 update-kubeconfig --name my-cluster

3. Declare evn for DB
    kubectl apply -f pvc.yaml
    kubectl apply -f pv.yaml

    Deploy database
        kubectl apply -f postgresql-deployment.yaml

    Config service DB
        kubectl apply -f postgresql-service.yaml

4. Set up dummy data
    Forward container in kube to port 5433 local to execute script create and insert dummy data
        kubectl port-forward svc/postgresql-service 5433:5432 &
    
    Run script create table
        psql --host 127.0.0.1 -U myuser -d mydatabase -p 5433 < 1_create_tables.sql

    Insert dummy data
        psql --host 127.0.0.1 -U myuser -d mydatabase -p 5433 < 1_create_tables.sql
        psql --host 127.0.0.1 -U myuser -d mydatabase -p 5433 < 2_seed_users.sql
        psql --host 127.0.0.1 -U myuser -d mydatabase -p 5433 < 3_seed_tokens.sql

    Check data whether insert or not by
        psql --host 127.0.0.1 -U myuser -d mydatabase -p 5432
        select *from users;
        select* from tokens;

5. Deploy application from ECR
    Config parameter
        kubectl apply -f configmap.yaml
    
    Deploy and set services
        kubectl apply -f coworking.yaml
6. Double check
    kubectl get services
    kubectl get pods

7. Visit expose of load balancer to check app
    <exposeELB link>:5153/api/reports/daily_usage
    <exposeELB link>:5153/api/reports/user_visits

8. Finish and delete kube cluster
    eksctl delete cluster --name my-cluster --region us-east-1
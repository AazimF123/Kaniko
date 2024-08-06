# Kaniko
 1. step 1: download jenkins using helm:
    
     - helm install jenkins jenkins/jenkins
     - default username: admin 
     - default password is given by applying this cmd line: 
       printf $(kubectl get secret --namespace default jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 –decode);echo

 2. step2: get the default values file using the following cmd line:

     - helm show values jenkins/jenkins > jenkins-new-values.yaml

 3. Step3: edit the values file:
    
     - use this link for help - https://github.com/sglvt/k8s-kaniko-jenkins/blob/master/values.yaml
       for this example I changed the pod template to create a kaniko pod template:
       To apply your changes: helm upgrade <RELEASE_NAME> <CHART_NAME> --values /path/to/values.yaml



 5. Step4: create a local registry and kaniko secret (adjust the permission of the file accordingly for all yaml files):

   - Docker_Registry_deployment.yaml:
   - Registory-service.yaml
   - create a config.JSON file to access docker registory:

    you can get you auth by: echo -n 'myuser:mypassword' | base64  
    after to create your kaniko–secret you use the following cmd:  kubectl create secret generic kaniko-secret --from-file=config.json=./config.json

Summary:

    • The config.json file is needed to provide authentication credentials to your Docker registry.
    • Creating a Kubernetes secret to store this file securely allows Kaniko to build and push images without exposing sensitive information.
    • This approach enhances security and integrates well with Kubernetes' native secret management.
    for more information use this link for help: https://medium.com/geekculture/deploying-docker-registry-on-kubernetes-3319622b8f32




5. Step5: create service account yaml file and role binding yaml file:

     -- apply it: kubectl apply –f service-example.yaml
     -- apply it: kubectl apply –f rolebinding-example.yaml

6. create a Dockerfile and application code (app.py) and requirements.txt to list the dependencies needed.




  Step 1: download jenkins using helm:
          
        - helm install jenkins jenkins/jenkins
        - default username: admin 
        - default password is given by applying this cmd line: 
          printf $(kubectl get secret --namespace default jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 –decode);echo

  Step 2: Get the default values file using the following cmd line:
  
        - helm show values jenkins/jenkins > jenkins-new-values.yaml
    
  Step 3: edit the values file:
  
        - Use this link for help - https://github.com/sglvt/k8s-kaniko-jenkins/blob/master/values.yaml
        - For this example I changed the pod template to create a kaniko pod template:
  ![image](https://github.com/user-attachments/assets/f8b7f92f-6814-4e75-acc3-90a313d36c9e)

        - To apply your changes: helm upgrade <RELEASE_NAME> <CHART_NAME> --values /path/to/values.yaml





  Step 4: Create a local registry and kaniko secret (adjust the permission of the file accordingly for all yaml files):

        - Docker_Registry_deployment.yaml:
   ![image](https://github.com/user-attachments/assets/21131207-5d95-4582-b459-b67baef4f5f3)


        - Registory-service.yaml:
   ![image](https://github.com/user-attachments/assets/f78c7876-3047-4987-99b0-97fbdc97e876)


        - create a config.JSON file to access docker registory:
   ![image](https://github.com/user-attachments/assets/806e878b-bff5-43b4-a5b4-c5205ab028af)
       
        - you can get you auth by: echo -n 'myuser:mypassword' | base64  
      
        - create kaniko-secret.yaml:
   ![image](https://github.com/user-attachments/assets/e6d1a541-46cd-4b31-a44b-b0073679096e)

        - To create kaniko-secret:  kubectl create secret generic kaniko-secret --from-file=config.json=./config.json

Summary:

    • The config.json file is needed to provide authentication credentials to your Docker registry.
    • Creating a Kubernetes secret to store this file securely allows Kaniko to build and push images without exposing sensitive information.
    • This approach enhances security and integrates well with Kubernetes' native secret management.
    for more information use this link for help: https://medium.com/geekculture/deploying-docker-registry-on-kubernetes-3319622b8f32









    5. Step5: create service account yaml file and role binding yaml file:

-- apply it: kubectl apply –f service-example.yaml
-- apply it: kubectl apply –f rolebinding-example.yaml
    6. create a Dockerfile and application code (app.py) and requirements.txt to list the dependencies needed.




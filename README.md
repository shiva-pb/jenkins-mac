docker network create jenkins

docker run -d \
  --name jenkins \
  --restart=unless-stopped \
  -p 8080:8080 \
  -p 50000:50000 \
  -v /Users/shivapb/Applications/jenkins:/var/jenkins_home \
  --network jenkins \
  jenkins/jenkins:lts-jdk17

docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword

#####
kubectl apply -f jenkins-pv.yaml #Cluster-wide resource
kubectl apply -f jenkins-pvc.yaml #Namespace-scoped resource

helm repo add jenkins https://charts.jenkins.io
helm repo update

helm install jenkins jenkins/jenkins -f jenkins-values.yaml -n tools --create-namespace

helm upgrade --install jenkins jenkins/jenkins -n tools -f jenkins-values.yaml --create-namespace

#####
***************** Sonar Qube installation *******************
yum install unzip wget
adduser sonarqube
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.4.0.54424.zip
unzip *
chmod -R 755 /home/sonarqube/sonarqube-9.4.0.54424
chown -R sonarqube:sonarqube /home/sonarqube/sonarqube-9.4.0.54424
cd sonarqube-9.4.0.54424/bin/linux-x86-64/
./sonar.sh start
**************************************************************
************ Argocd cli installation **************************************
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
ln -s /usr/local/bin/argocd /bin/argocd

argocd login 34.123.254.239:80 --username admin

**************************************************************************

gcloud config configurations create svacc
gcloud iam roles describe roles/container.admin
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

kubectl get scv -n argocd
kubectl get secret argocd-initial-admin-secret -n argocd -o yaml


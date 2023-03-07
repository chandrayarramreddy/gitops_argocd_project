pipeline{
    agent any

    stages{

        stage("Run shell"){

            steps{
                    
                  //  sh "sudo chown -R jenkins:chandra /var/lib/jenkins/"
                  //  sh "docker ps"
                   // sh "ll"
                   // sh "export MINIKUBE_HOME=/home/chandra/.minikube"
                   // sh "kubectl get pods"
                    //sh "kubectl get pods"
                    sh "curl -L http://192.168.49.2:30007/demo/"
            }
        }
    }
}
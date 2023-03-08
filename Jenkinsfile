pipeline{
    agent any

    environment {
        DOCKERHUB_USERNAME = "chandray"
        APP_NAME = "argo-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}"+"/"+"${APP_NAME}"
        REGISTRY_CREDS = 'dockerhub'
    } //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

    stages{

        stage("Run shell"){

            steps{
                    
                  //  sh "sudo chown -R jenkins:chandra /var/lib/jenkins/"
                    sh "docker images"
                   // sh "ll"
                   // sh "export MINIKUBE_HOME=/home/chandra/.minikube"
                   // sh "kubectl get pods"
                    sh "kubectl get pods"
                  //  sh "curl -L http://192.168.49.2:30007/demo/ -v"
            }

        }

        stage("Checkout SCM"){

            steps{
                script{
                    git credentialsId: 'github',
                    url: 'https://github.com/chandrayarramreddy/gitops_argocd_project.git',
                    branch: 'main'
                }  //windows  - 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
            }
        }

        stage("Build Docker Image"){
            steps{
                script{
                   // sh "sudo usermod -a -G docker jenkins"
                    docker_image = docker.build "${IMAGE_NAME}"
                }
            }
        }

        stage("Push Docker Image"){
            steps{
                script{
                    docker.withRegistry('',REGISTRY_CREDS){
                        docker_image.push("$BUILD_NUMBER")
                        docker_image.push('latest')
                    }
                }
            }
        }

        stage("Delete DOcker Image"){
            steps{
                script{
                    sh"docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh"docker rmi ${IMAGE_NAME}:latest"
                }
            }
        }

        stage("Updating K8s deploy version"){
            steps{
                script{
                    sh"""
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                    """
                }
            }
        }

        stage("Push Changes of Deployment file to Git"){
            steps{
                script{
                    sh """
                      git config --global user.name "chandra Jenkins User"
                      git config --global user.email "chandra.y@gmail.com"
                      git add deployment.yaml
                      git commit -m "Updated Deployment file"
                    """
                    withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                     sh" git push https://github.com/chandrayarramreddy/gitops_argocd_project.git main"
                    }
                }
            }
        }

    }
}
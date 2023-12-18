pipeline {
    agent { label 'Slave-Node'}
    tools{
        maven 'Maven'
    }
    environment {
        APP_NAME = "container-cicd"
        RELEASE = "1.0.0"
        DOCKER_USER = "ganeshmete11@gmail.com"
        DOCKER_PASS = "DockerId"
        IMAGE_NAME  = "ganeshmete11" + "/" + "${APP_NAME}"
        IMAGE_TAG   = "${RELEASE}-${BUILD_NUMBER}"
    }

    stages{
        stage("Cleanup Workspace"){
            steps{
                cleanWs()
            }
        }
        stage("Checkout from SCM"){
            steps{
                git branch: 'main', credentialsId: 'Github', url: 'https://github.com/e5664869/container-cicd.git'
            }
        }
        stage("Build Application"){
            steps{
                sh "mvn clean package"
            }
        }
        stage("Test Application"){
            steps{
                sh "mvn test"
            }
        } 
        
        stage("SonarQube Analysis"){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'SonarQube') {
                        sh "mvn sonar:sonar"
                    }
                }
            }
        }
        /*
        stage("Quality Gate Report"){
            steps{
                script{
                    timeout(time: 2, unit: 'MINUTES'){
                        waitForQualityGate abortPipeline: false, credentialsId: 'SonarQube'
                    }    
                }
            }
        } 
        */
        stage("Build & Push Docker Image"){
            steps{
                script{
                    withDockerRegistry(credentialsId: 'DockerId') {
                        docker_image = docker.build "${IMAGE_NAME}"
    
                    }
                    withDockerRegistry(credentialsId: 'DockerId') {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }
        
        }
        stage("Trivy Scan"){
            steps{
                script{
                    sh ('docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image ganeshmete11/container-cicd:latest --no-progress --scanners vuln  --exit-code 0 --severity HIGH,CRITICAL --format table')
                    
                }
            }
        }
        stage("Cleanup artifact"){
            steps{
                script{
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
                }
            }
        }
        
    }
}
    
pipeline {
    agent { label 'Slave-Node'}
    tools{
        maven 'Maven'
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
        stage("Quality Gate Report"){
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'SonarQube'
                    
                }
            }
        } 

    }

}
    
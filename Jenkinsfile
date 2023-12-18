pipeline {
    agent { label 'Slave-Node'}

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
        /*
        stage("Test Application"){
            steps{
                sh "mvn test"
            }
        } */

    }

}
    
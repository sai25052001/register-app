pipeline {
    agent {
        label 'Jenkins-Agent'
    }
    tools{
        jdk 'Java17'
        maven 'Maven3'
    }

    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout from the SCM'){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/sai25052001/register-app.git'
            }
        }
        stage('Build Application'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('Test Application'){
            steps{
                echo 'mvn test'
            }
        }
        stage('sonar qube Analysis'){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token'){
                        sh 'mvn sonar:sonar'
                    }
                }
            }
        }
        stage('Quality Gate'){
            steps{
                script{
                    waitForQualityGate abortPipeine: false, credentialsId: 'jenkins-sonarqube-token'
                }
            }
        }
    }
}

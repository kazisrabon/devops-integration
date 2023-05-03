pipeline {
    agent any

    tools {
        // Install the Maven
        maven "3.8.7"
    }

    stages {
        stage('Build Maven') {
            steps {
                // Get some code from a GitHub repository
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/kazisrabon/devops-integration']])

                // Run Maven on a Unix agent.
                sh "mvn clean install"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }

        stage("Build Docker Image"){
            steps{
                script{
                    sh "docker build -t kazisrabon/devops-integration ."
                }
            }
        }
        stage("Push Image to Hub"){
            steps{
                script{
                    withCredentials([string(credentialsId: 'docker-hub-pass', variable: 'dockerhubpass')]) {
                        sh "docker login -u kazisrabon -p ${dockerhubpass}"
                    }
                    sh "docker push kazisrabon/devops-integration"
                }
            }
        }
    }
}

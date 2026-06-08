pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'mvn compile'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Package') {
            steps {
                bat 'mvn package'
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t maven-demo:latest .'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'

                    bat "docker tag maven-demo:latest rajmandhyan/maven-demo:%BUILD_NUMBER%"
                    bat "docker push rajmandhyan/maven-demo:%BUILD_NUMBER%"

                    bat "docker tag maven-demo:latest rajmandhyan/maven-demo:latest"
                    bat "docker push rajmandhyan/maven-demo:latest"
                }
            }
        }

        stage('Docker Run') {
            steps {
                bat 'docker run --rm maven-demo:latest'
            }
        }
        
        stage('Deploy') {
            steps {
                bat 'dir target'
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'target/*.jar'
            echo 'Artifact Archived Successfully'
        }
    }
}
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
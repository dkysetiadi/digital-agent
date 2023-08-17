pipeline {
    agent {
        label 'docker'
    }
    environment {
        GCP_SERVICE_ACCOUNT ('SERVICE_ACCOUNT_GCP')
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh 'docker build -t gcr.io/ferrous-module-395010/digital-agent:${BUILD_NUMBER} .'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}

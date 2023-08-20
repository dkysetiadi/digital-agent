pipeline {
    agent {
        label 'docker'
    }
    environment {
        GCP_SERVICE_ACCOUNT = credentials('SERVICE_ACCOUNT_GCP')
        GCP_SSH_KEY = credentials('GCP_PRIVATE_KEY')
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh 'docker build -t gcr.io/ferrous-module-395010/digital-agent:${BUILD_NUMBER} .'
            }
        }
        stage('Push') {
            steps {
                echo 'Testing..'
                sh 'cat "$GCP_SERVICE_ACCOUNT" | docker login -u _json_key --password-stdin https://gcr.io'  
                sh 'docker push gcr.io/ferrous-module-395010/digital-agent:${BUILD_NUMBER}'
            }
        }
        stage('Active GCP Account') {
            steps {
                echo 'Active Account'
                sh 'ssh -o StrictHostKeyChecking=no -i "$GCP_SSH_KEY" dickysetiadi64@34.101.98.183 "whoami"'
                sh 'ssh -o StrictHostKeyChecking=no -i "$GCP_SSH_KEY" dickysetiadi64@34.101.98.183 "gcloud auth list"'
                sh 'ssh -o StrictHostKeyChecking=no -i "$GCP_SSH_KEY" dickysetiadi64@34.101.98.183 "gcloud container clusters get-credentials cluster-jenkins --zone asia-southeast2-a --project ferrous-module-395010"'
                sh 'ssh -o StrictHostKeyChecking=no -i "$GCP_SSH_KEY" dickysetiadi64@34.101.98.183 "kubectl get nodes"'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
                sh 'helm repo add dkysetiadi-charts https://adhithia21.github.io/helm-charts/charts'
                sh 'helm repo list'
                sh 'helm upgrade --install goapp dkysetiadi-charts/application
            }
        }
    }
}

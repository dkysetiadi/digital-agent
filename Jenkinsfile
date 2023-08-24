pipeline {
    agent {
        label 'docker'
    }
    environment {
        GCP_SERVICE_ACCOUNT = credentials('SERVICE_ACCOUNT_GCP')
        GCP_SSH_KEY = credentials('GCP_PRIVATE_KEY')
        KUBE_CONFIG = credentials('KUBE_CONFIG')
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
        stage('Deploy') {
            steps {
                echo 'Deploying....'
                sh 'ssh -o StrictHostKeyChecking=no -i "$GCP_SSH_KEY" dickysetiadi64@34.101.98.183 "whoami"'
                sh 'ssh -o StrictHostKeyChecking=no -i "$GCP_SSH_KEY" dickysetiadi64@34.101.98.183 "gcloud auth list"'
                sh 'ssh -o StrictHostKeyChecking=no -i "$GCP_SSH_KEY" dickysetiadi64@34.101.98.183 "gcloud container clusters get-credentials cluster-jenkins --zone asia-southeast2-a --project ferrous-module-395010"'
                sh 'ssh -o StrictHostKeyChecking=no -i "$GCP_SSH_KEY" dickysetiadi64@34.101.98.183 "kubectl get nodes"'
                sh 'ssh -o StrictHostKeyChecking=no -i "$GCP_SSH_KEY" dickysetiadi64@34.101.98.183 "helm repo add dkysetiadi-charts https://adhithia21.github.io/helm-charts/charts"'
                // sh 'ssh -o StrictHostKeyChecking=no -i "$GCP_SSH_KEY" dickysetiadi64@34.101.98.183 "helm upgrade --install digital-agent dkysetiadi-charts/application --set image=gcr.io/ferrous-module-395010/digital-agent:${BUILD_NUMBER}"'                
            }
        }
        // stage('Deploy') {
        //     steps {
        //         echo 'Deploying....'
        //     }
        // }
    }
}

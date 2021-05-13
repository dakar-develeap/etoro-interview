pipeline {
    agent any

    stages {
        stage('Chekcout') {
            steps {
                checkout scm
            }
        }
        stage('Set Version') {
            steps {
                echo 'Setting version..'
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                az login --identity --output none
                az acr login --name $ACR_LOGINSERVER
                az aks get-credentials --resource-group $AKS_RESOURCE_GROUP --name $AKS_CLUSTER
                helm upgrade --install --namespace default simple-web-chart dakar-etoro-chart/
                '''
            }
        }
    }
    post {
        always {
            deleteDir()
        }
        success {
            echo 'Successful ETORO'
        }
        unstable {
            echo 'Unstable ETORO'
        }
        failure {
            echo 'Failed ETORO'
        }        
    }
}

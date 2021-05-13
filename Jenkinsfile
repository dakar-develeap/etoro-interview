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
                az login --identity
                az acr login --name $ACR_LOGINSERVER
                az aks get-credentials --resource-group $AKS_RESOURCE_GROUP --name $AKS_CLUSTER
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

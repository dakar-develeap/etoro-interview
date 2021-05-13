properties([pipelineTriggers([githubPush()])])

pipeline {
    agent any

    stages {
        stage('Chekcout') {
            when {
                changeset "dakar-etoro-chart/**"
            }            
            steps {
                checkout scm
            }
        }
        stage('Deploy') {
            when {
                changeset "dakar-etoro-chart/**"
            }            
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

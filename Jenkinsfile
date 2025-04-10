pipeline {
    agent any

    environment {
        AZURE_CREDENTIALS_ID = 'react-jenkins-principle'
        RESOURCE_GROUP = 'rg-jenkins'
        APP_SERVICE_NAME = 'webapiyashpjenkins82648'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/yash8209/reactjs2_terraform_jenkins_pipeline.git'
            }
        }

        stage('Terraform Init/Plan/Apply') {
            steps {
                dir('terraform2233') {
                    bat 'terraform init'
                    bat 'terraform plan -out=tfplan'
                    bat 'terraform apply -auto-approve tfplan'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                dir('ikeasoftwares') {
                    bat 'npm install'
                }
            }
        }

        stage('Build React App') {
            steps {
                dir('ikeasoftwares') {
                    bat 'npm run build'
                }
            }
        }

        stage('Prepare Build Zip') {
            steps {
                bat '''powershell -Command "Compress-Archive -Path ikeasoftwares\\build\\* -DestinationPath build.zip -Force"'''
            }
        }

        stage('Deploy to Azure') {
            steps {
                withCredentials([azureServicePrincipal(credentialsId: AZURE_CREDENTIALS_ID)]) {
                    bat "az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET --tenant $AZURE_TENANT_ID"
                    bat "az webapp deployment source config-zip --resource-group $RESOURCE_GROUP --name $APP_SERVICE_NAME --src build.zip"
                }
            }
        }
    }

    post {
        success {
            echo '✅ React App Provisioned and Deployed Successfully!'
        }
        failure {
            echo '❌ Build or Deployment Failed!'
        }
    }
}




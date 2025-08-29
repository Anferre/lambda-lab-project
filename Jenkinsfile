pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'gitlab-private-key', url: 'git@github.com:Anferre/lambda-lab-project.git'
            }
        }
        stage('Build e Empacotamento') {
            steps {
                sh 'npm install'
                sh 'zip -r lambda-code.zip .'
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def functionName = "lambda-lab-test-deploy"
                    def region = "us-east-1"
                    sh "aws lambda update-function-code --function-name ${functionName} --zip-file fileb://lambda-code.zip --region ${region}"
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
        success {
            echo '✅ Deploy concluído com sucesso!'
        }
        failure {
            echo '❌ Falha no deploy.'
        }
    }
}
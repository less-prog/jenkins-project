pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'GITHUB_CREDENTIALS', variable: 'GIT_TOKEN')]) {
                        sh 'git clone https://$GIT_TOKEN@github.com/less-prog/jenkins-project.git'
                    }
                }
                sh "ls -ltr"
            }
        }

        stage('Setup') {
            steps {
                sh "pip install -r requirements.txt"
            }
        }

        stage('Test') {
            steps {
                sh "pytest"
                sh "whoami"
            }
        }
    }

    post {
        always {
            script {
                echo "Pipeline terminé, nettoyage en cours..."
            }
        }
    }
}

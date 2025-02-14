pipeline {
    agent any  // Ou utiliser 'agent { label "python" }' selon la config de Jenkins

    environment {
        VENV_DIR = 'venv'  // Répertoire pour l’environnement virtuel Python
    }

    stages {

        stage('Checkout') {
            steps {
                git url: 'https://github.com/less-prog/jenkins-project.git', branch: 'main'
                sh "ls -ltr"
            }
        }

        stage('Setup') {
            steps {
                sh """
                python3 -m venv $VENV_DIR
                source $VENV_DIR/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                """
            }
        }

        stage('Test') {
            steps {
                script {
                    catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
                        sh """
                        source $VENV_DIR/bin/activate
                        pytest --maxfail=5 --disable-warnings
                        """
                    }
                    sh "whoami"
                }
            }
        }
    }

    post {
        always {
            sh "deactivate || true"  // Désactive l’environnement virtuel si activé
            sh "rm -rf $VENV_DIR || true"  // Nettoyage
        }
        failure {
            echo "❌ Le pipeline a échoué ! Vérifiez les logs."
        }
        success {
            echo "✅ Tests passés avec succès !"
        }
    }
}

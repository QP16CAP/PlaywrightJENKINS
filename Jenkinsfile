pipeline {
    agent {
        docker {
            image 'playwright/chromium:playwright-1.56.1'
            args '--user=root --entrypoint=""'
        }
    }

    stages {
        stage('Démarrage configuration du projet') {
            steps {

                // Supprimer l'ancien repo
                sh 'rm -rf repo'

                // Cloner le repo
                sh 'git clone https://github.com/QP16CAP/PlaywrightJENKINS.git repo'

                // Vérifier les versions Node et Playwright
                sh 'node -v'
                sh 'npx playwright --version'

                // Accéder au dossier repo et exécuter les tests
                dir('repo') {
                    sh 'npm install'
                    sh 'npx playwright test --project=chromium'
                }
            }
        }
    }
}

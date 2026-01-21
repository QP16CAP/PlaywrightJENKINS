pipeline {
    agent {
        docker {
            image 'playwright/chromium:playwright-1.56.1'
            args '--user=root --entrypoint=""'
        }
    }

    stages {
        stage('Préparation du projet') {
            steps {
                // Installer git
                sh 'apt-get update && apt-get install -y git'

                // Supprimer l'ancien repo si présent
                sh 'rm -rf repo'

                // Cloner le repo
                sh 'git clone https://github.com/QP16CAP/PlaywrightJENKINS.git repo'

                // Installer les dépendances et navigateurs Playwright
                dir('repo') {
                    sh 'npm install'
                    sh 'npx playwright install'
                }
            }
        }

        stage('Exécution des tests Playwright') {
            steps {
                dir('repo') {
                    sh 'node -v'
                    sh 'npx playwright --version'
                    sh 'npx playwright test --project=chromium'
                }
            }
        }
    }
}

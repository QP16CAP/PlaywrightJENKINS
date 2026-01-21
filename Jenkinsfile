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
                // Supprimer l'ancien repo
                sh 'rm -rf repo'

                // Cloner le repo
                sh 'git clone https://github.com/QP16CAP/PlaywrightJENKINS.git repo'
            }
        }

        stage('Installation des dépendances') {
            steps {
                // Aller dans le dossier repo
                dir('repo') {
                    sh 'npm install'
                    // Installer les navigateurs Playwright
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

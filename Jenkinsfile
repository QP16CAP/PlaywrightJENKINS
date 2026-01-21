pipeline {
    agent {
        docker {
            image 'playwright/chromium:playwright-1.56.1'
            args '--user=root --entrypoint=""'
        }
    }

    stages {
        stage('Installation des dépendances') {
            steps {
                dir("${env.WORKSPACE}") {
                    // Installer les dépendances Node du projet
                    sh 'npm install'
                }
            }
        }

        stage('Vérification et tests Playwright') {
            steps {
                dir("${env.WORKSPACE}") {
                    // Vérifier les versions Node et Playwright
                    sh 'node -v'
                    sh 'npx playwright --version'

                    // Lancer les tests Playwright pour Chromium
                    sh 'npx playwright test --project=chromium'
                }
            }
        }
    }
}

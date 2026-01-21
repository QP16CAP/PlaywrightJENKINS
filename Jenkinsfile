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
                // Installer npm et les navigateurs Playwright dans le repo déjà cloné
                sh 'npm install'
                sh 'npx playwright install'
            }
        }

        stage('Exécution des tests Playwright') {
            steps {
                sh 'node -v'
                sh 'npx playwright --version'
                sh 'npx playwright test --project=chromium'
            }
        }
    }
}

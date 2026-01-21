pipeline {
    agent any

    stages {
        stage('Clonage du projet') {
            steps {
                sh 'rm -rf repo'
                git url: 'https://github.com/QP16CAP/PlaywrightJENKINS.git', branch: 'main'
            }
        }

        stage('Exécution des tests Playwright') {
            agent {
                docker {
                    image 'playwright/chromium:playwright-1.56.1'
                    args '--user=root --entrypoint=""'
                }
            }
            steps {
                dir('PlaywrightJENKINS') { // nom du dossier cloné
                    sh 'npm install'
                    sh 'npx playwright test --project=chromium'
                }
            }
        }
    }
}

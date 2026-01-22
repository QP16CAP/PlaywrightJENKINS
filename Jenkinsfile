pipeline {
    agent {
        docker {
            image 'mcr.microsoft.com/playwright:v1.57.0-noble'
            args '--user=root --entrypoint="" -v $WORKSPACE/allure-results:/allure-results'
        }
    }

    parameters {
        choice(name: 'Navigateur', choices: ['chromium', 'webkit', 'firefox'], description: 'Sélectionner un navigateur')
        choice(name: 'tag', choices:['invalide','valide'])
    }

    stages {
        stage('Préparation du projet') {
            steps {
                sh 'node -v'
                sh 'npx playwright --version'
                sh 'rm -rf repo'
                sh 'git clone https://github.com/QP16CAP/PlaywrightJENKINS.git repo'

                dir('repo') {
                    sh 'npm ci'
                }
            }
        }

        stage('Exécution des tests Playwright') {
            steps {
                dir('repo') {
                    // Nettoyer ancien rapport
                    sh 'rm -rf allure-results'
                    
                    script {
                        if (params.Navigateur == 'chromium') {
                            sh 'npx playwright test --project=chromium --reporter=allure-playwright'
                            // Copier les résultats vers le volume partagé
                            sh 'cp -r allure-results/* /allure-results/'
                        } else {
                            error 'Navigateur non valide sélectionné'
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            // Génération du rapport Allure sur l’hôte Jenkins
            allure includeProperties: false, jdk: '', results: [[path: 'allure-results']]
        }
    }
}

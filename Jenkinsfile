pipeline {
    agent {
        docker {
            image 'mcr.microsoft.com/playwright:v1.57.0-noble'
            args '--user=root --entrypoint=""'
        }
    }

    parameters {
        choice(
            name: 'Navigateur',
            choices: ['chromium', 'webkit', 'firefox'],
            description: 'Sélectionner un navigateur pour le test',
        )
        choice(name: 'tag', choices:['invalide','valide'])
    }

    stages {

        stage('Préparation du projet') {
            steps {
                sh 'node -v'
                sh 'npx playwright --version'

                //sh 'apt-get update && apt-get install -y git'

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
                    script {
                        // Nettoyage ancien rapport Allure
                        sh 'rm -rf allure-results'

                        if (params.Navigateur == 'chromium') {
                            sh 'npx playwright test --project=chromium'
                        } else {
                            error 'Navigateur non valide sélectionné'
                        }
                    }
                }
            }
        }
    }

    post {
        success{
            script {
                if(params.tag == 'valide'){
                    build job: 'login'
                }else {
                    sh 'echo "le tags choisi est valide"'
                }
                //lancer rapport allure apres le succes des tests
                script {
                    dir('repo') {
                        sh 'npx allure generate allure-results --clean -o allure-report || true'
                        sh 'npx allure open allure-report'
                    }
                }
            }
        }
    }

}

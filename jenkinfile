pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Specify your Git credentials ID here
                    def gitCredentialsId = 'your-git-credentials-id'

                    // Use the 'withCredentials' block to provide Git credentials
                    withCredentials([usernamePassword(
                        credentialsId: gitCredentialsId,
                        usernameVariable: 'GIT_USERNAME',
                        passwordVariable: 'GIT_PASSWORD'
                    )]) {
                        // Check out the code with provided credentials
                        checkout([
                            $class: 'GitSCM',
                            branches: [[name: '*/master']],
                            doGenerateSubmoduleConfigurations: false,
                            extensions: [
                                [$class: 'CleanBeforeCheckout'],
                                [$class: 'CloneOption',
                                    depth: 1,
                                    noTags: true,
                                    reference: '',
                                    shallow: true,
                                    timeout: 120
                                ]
                            ],
                            submoduleCfg: [],
                            userRemoteConfigs: [[
                                credentialsId: gitCredentialsId,
                                url: 'https://github.com/your/repo.git'
                            ]]
                        ])
                    }
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'composer install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'php artisan test'
            }
        }

        stage('Build and Deploy') {
            steps {
                sh 'php artisan optimize'
                sh 'npm install'
                sh 'npm run production'
                // Add deployment steps here
            }
        }
    }
}


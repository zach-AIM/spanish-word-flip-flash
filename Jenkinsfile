pipeline {
    agent any
    
    options {
        ansiColor('xterm')
    }

    stages {
        stage('build') {
            agent {
                docker {
                    image 'node:22-alpine'
                }
            }
            steps {
                sh 'npm ci'
                sh 'npm run build'
            }
        }

        stage('test') {
            parallel {
                stage('unit tests') {
                    agent {
                        docker {
                            image 'node:22-alpine'
                            reuseNode true
                        }
                    }
                    steps {
                        // Unit tests with Vitest
                        sh 'npx vitest run --reporter=verbose'
                    }
                }
                stage('integration tests') {
                    agent {
                        docker {
                            image 'mcr.microsoft.com/playwright:v1.54.2-jammy'
                            reuseNode true
                        }
                    }
                    steps {
                        // Integration tests with Playwright
                        sh 'npx playwright test --reporter=html'
                    }
                }
            }
        }

        stage('deploy') {
            agent {
                docker {
                    image 'alpine'
                }
            }
            steps {
                // Mock deployment which does nothing
                echo 'Mock deployment was successful!'
            }
        }

        stage('e2e') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.54.2-jammy'
                }
            }
            environment {
                // Set environment variables for Playwright tests if needed
                E2E_BASE_URL = 'https://spanish-cartds.netlify.app'
            }
            steps {
                // End-to-end tests with Playwright
                sh 'npx playwright test --reporter=html'
            }
        }
    }
}
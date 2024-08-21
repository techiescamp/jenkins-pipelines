pipeline {
    agent {
        kubernetes {
            yaml '''
                apiVersion: v1
                kind: Pod
                spec:
                  containers:
                  - name: maven
                    image: maven:3.8.4-openjdk-17
                    command:
                    - cat
                    tty: true
            '''
        }
    }
    options {
        buildDiscarder logRotator( 
                    daysToKeepStr: '16', 
                    numToKeepStr: '10'
            )
    }

    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
                sh "echo 'Cleaned Up Workspace For Project'"
            }
        }

        stage('Code Checkout') {
            steps {
                checkout scm
            }
        }

        stage('PR Build and Test') {
            when {
                changeRequest()
            }
            stages {
                stage('Unit Testing') {
                    steps {
                        sh "echo 'Running Unit Tests for PR'"
                        // Add actual unit test commands here
                    }
                }

                stage('Code Analysis') {
                    steps {
                        sh "echo 'Running Code Analysis for PR'"
                        // Add actual code analysis commands here
                    }
                }

                stage('Build Code') {
                    steps {
                        sh "echo 'Building Artifact for PR'"
                        // Add actual build commands here
                    }
                }
            }
        }

        stage('Develop Branch Tasks') {
            when {
                allOf {
                    branch 'develop'
                    not { changeRequest() }
                }
            }
            stages {
                stage('Build for Develop') {
                    steps {
                        sh "echo 'Building for Develop'"
                        // Add develop build steps
                    }
                }
                stage('Deploy to Dev Environment') {
                    steps {
                        sh "echo 'Deploying to Dev Environment'"
                        // Add dev deployment steps
                    }
                }
            }
        }

        stage('Main Branch Tasks') {
            when {
                allOf {
                    branch 'main'
                    not { changeRequest() }
                }
            }
            stages {
                stage('Build for Production') {
                    steps {
                        sh "echo 'Building for Production'"
                        // Add production build steps
                    }
                }
                stage('Deploy to Pre-prod') {
                    steps {
                        sh "echo 'Deploying to Pre-prod Environment'"
                        // Add pre-prod deployment steps
                    }
                }
            }
        }
    }

    post {
        always {
            sh "echo 'Pipeline Completed'"
            // Add any cleanup or notification steps here
        }
    }
}

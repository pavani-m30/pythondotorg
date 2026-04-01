pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/pavani-m30/pythondotorg.git'
            }
        }

        stage('Setup Python') {
            steps {
                sh '''
                    pip3 install -r requirements.txt || true
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                    pytest || true
                '''
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONARQUBE = credentials('sonar-token-id')
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                        sonar-scanner \
                          -Dsonar.projecectKey=pipeline \
                          -Dsonar.sources=. \
                          -Dsonar.host.url=http://localhost:9000 \
                          -Dsonar.login=$SONARQUBE
                    '''
                }
            }
        }
    }
}

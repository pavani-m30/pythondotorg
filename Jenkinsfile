pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/pavani-m30/pythondotorg.git'
            }
        }

        stage('Run Python Script') {
            steps {
                sh '''
                    # Run your Python file directly
                    python3 your_script.py
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
                          -Dsonar.projectKey=python-project \
                          -Dsonar.sources=. \
                          -Dsonar.host.url=http://localhost:9000 \
                          -Dsonar.login=$SONARQUBE
                    '''
                }
            }
        }
    }
}

pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh 'docker build -t dolfin-app .'
            }
        }
        
        stage('Test') {
            steps {
                echo 'Testing..'
                sh 'docker run dolfin-app pytest'
            }
        }
        
        stage('Code Quality Analysis') {
            steps {
                echo 'Running Code Quality Analysis..'
                sh 'sonar-scanner'
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying to staging..'
                sh 'docker-compose up -d'
            }
        }
        
        stage('Release') {
            steps {
                input message: 'Promote to production?', ok: 'Release'
                echo 'Releasing to production..'
                sh 'docker-compose -f docker-compose.prod.yml up -d'
            }
        }
        
        stage('Monitoring and Alerting') {
            steps {
                echo 'Setting up monitoring..'
                // Add your monitoring setup scripts or commands here
            }
        }
    }
}

pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package' // Adjust command according to your build tool
            }
            post {
                success {
                    archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
                }
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
        stage('Code Quality Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Deploy to Test') {
            steps {
                sh 'docker-compose up -d'
            }
        }
        stage('Release to Production') {
            steps {
                sh 'aws deploy create-deployment --application-name MyApp --deployment-group-name MyDeploymentGroup --s3-location bucket=mybucket,key=mykey,bundleType=zip'
            }
        }
        stage('Monitoring and Alerting') {
            steps {
                sh 'datadog-agent status'
            }
        }
    }
}

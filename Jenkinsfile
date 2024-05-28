pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    try {
                        echo 'Building...'
                        // Use Windows-compatible commands
                        bat 'mvn clean install' // Example for a Maven project
                    } catch (Exception e) {
                        echo "Build stage failed: ${e.message}"
                        currentBuild.result = 'FAILURE'
                        error("Build stage failed")
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    if (currentBuild.result == 'SUCCESS') {
                        try {
                            echo 'Testing...'
                            // Use Windows-compatible commands
                            bat 'mvn test' // Example for a Maven project
                        } catch (Exception e) {
                            echo "Test stage failed: ${e.message}"
                            currentBuild.result = 'FAILURE'
                            error("Test stage failed")
                        }
                    } else {
                        echo 'Skipping Test stage due to previous failure'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    if (currentBuild.result == 'SUCCESS') {
                        try {
                            echo 'Deploying...'
                            // Use Windows-compatible commands
                            bat 'powershell -Command "Copy-Item -Path target\\*.jar -Destination \\\\remote-server\\path\\to\\deploy"' // Example for deployment
                        } catch (Exception e) {
                            echo "Deploy stage failed: ${e.message}"
                            currentBuild.result = 'FAILURE'
                            error("Deploy stage failed")
                        }
                    } else {
                        echo 'Skipping Deploy stage due to previous failure'
                    }
                }
            }
        }

        stage('Release') {
            steps {
                script {
                    if (currentBuild.result == 'SUCCESS') {
                        try {
                            echo 'Releasing...'
                            // Use Windows-compatible commands
                            bat 'git tag -a v1.0.0 -m "Release version 1.0.0"' // Example for Git tagging
                            bat 'git push origin v1.0.0'
                        } catch (Exception e) {
                            echo "Release stage failed: ${e.message}"
                            currentBuild.result = 'FAILURE'
                            error("Release stage failed")
                        }
                    } else {
                        echo 'Skipping Release stage due to previous failure'
                    }
                }
            }
        }

        stage('Monitoring and Alerting') {
            steps {
                script {
                    if (currentBuild.result == 'SUCCESS') {
                        try {
                            echo 'Setting up Monitoring and Alerting...'
                            // Use Windows-compatible commands
                            def buildStatus = currentBuild.currentResult
                            emailext (
                                subject: "Jenkins Build ${buildStatus}: ${env.JOB_NAME} ${env.BUILD_NUMBER}",
                                body: "Jenkins Build ${buildStatus}: ${env.JOB_NAME} ${env.BUILD_NUMBER}\n\nCheck console output at ${env.BUILD_URL}",
                                to: 'your-email@example.com'
                            )
                        } catch (Exception e) {
                            echo "Monitoring and Alerting stage failed: ${e.message}"
                            currentBuild.result = 'FAILURE'
                            error("Monitoring and Alerting stage failed")
                        }
                    } else {
                        echo 'Skipping Monitoring and Alerting stage due to previous failure'
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            // Add cleanup steps if necessary
        }

        success {
            echo 'Pipeline succeeded!'
            // Add steps to run on success
        }

        failure {
            echo 'Pipeline failed!'
            // Add steps to run on failure
        }
    }
}

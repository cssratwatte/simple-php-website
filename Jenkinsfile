pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'aecf50a1-a868-4cec-91b2-9cc702dd955e'
        NETLIFY_AUTH_TOKEN = credentials('netlify-auth-token') // Assuming you have set up this credential in Jenkins
    }

    stages {
        stage('Build') {
            steps {
                script {
                    try {
                        echo 'Building...'
                        sh 'npm install' // Install dependencies
                        sh 'npm run build' // Build the React app
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
                            sh 'npm test' // Run tests
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
                            sh 'npm install netlify-cli -g' // Install Netlify CLI
                            sh 'netlify deploy --prod --dir=build --site=$NETLIFY_SITE_ID' // Deploy to Netlify
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
                            // Example: Git tagging for release
                            sh 'git tag -a v1.0.0 -m "Release version 1.0.0"'
                            sh 'git push origin v1.0.0'
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

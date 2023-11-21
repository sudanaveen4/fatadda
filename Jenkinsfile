pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {

                git 'https://github.com/sudanaveen4/fatadda.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    try {
                        // Set up environment variables
                        env.PATH = "${env.JAVA_HOME}/bin:${env.MAVEN_HOME}/bin:${env.PATH}"
                        
                        // Maven build with clean and install phases
                        sh 'mvn clean install'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        echo "Build failed: ${e.message}"
                    }
                }
            }
        }

        stage('Docker Build') {
            steps {
                // Build Docker image
                script {
                    dockerImage = docker.build("suda print")
                }
            }
        }

        stage('Docker Push') {
            steps {
                // Push Docker image to a registry
                script {
                    docker.withRegistry('https://hub.docker.com/repository/docker/sudanaveen4/suda_adda/general', '@20MIC0057s') {
                        dockerImage.push()
                    }
                }
            }
        }
    }

    post {
        failure {
            // Handle errors and print error messages
            echo 'Pipeline failed!'
            currentBuild.result = 'FAILURE'
        }
    }
}

pipeline {
    agent any

    environment {
        SONAR_SCANNER_HOME = tool 'SonarQube'
        // DOCKER_HUB_CREDENTIALS = credentials('mydocker')
        // imagename = 'laravel'
        // registry = 'docker.io'
        // imageTag = 'latest'
        // DOCKER_HUB_USERNAME = 'thato'

        REMOTE_USER = 'ubuntu'  // Replace with your EC2 instance's username
        SERVER_IP = '35.175.197.152'  // Replace this your EC2 instance's IP
        SSH_CREDENTIALS_ID = 'Sonar-Qube'
        GITHUB_REPO_URL = "https://github.com/SthandwaKay/SonarQube_project.git"

        // CONTAINER_NAME = 'laravel_app'  // Replace with your container name
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    checkout scm
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    sh "${SONAR_SCANNER_HOME}/bin/sonar-scanner -Dsonar.projectKey=SonarQube -Dsonar.sources=. -Dsonar.host.url=http://100.25.159.113:9000 -Dsonar.login=sqp_f7f09e8087e6aee20b962577f55298daa7e6e578"
                }
            }
        }

        // stage('Build Docker Image') {
        //     steps {
        //         script {
        //             // Build and tag Docker image
        //             sh "docker build -t ${registry}/${DOCKER_HUB_USERNAME}/${imagename}:${imageTag} -f users/Dockerfile ${WORKSPACE}/users"
        //         }
        //     }
        // }

        // stage('Trivy Scan') {
        //     steps {
        //         script {
        //             // Run Trivy Scan
        //             sh "trivy image ${registry}/${DOCKER_HUB_USERNAME}/${imagename}:${imageTag}"
        //         }
        //     }
        // }

        // stage('Push Docker Image to Docker Hub') {
        //     steps {
        //         script {
        //             // Log in to Docker Hub
        //             docker.withRegistry('https://index.docker.io/v1/', 'mydocker') {
        //                 // Push Docker image to Docker Hub
        //                 sh "docker push ${registry}/${DOCKER_HUB_USERNAME}/${imagename}:${imageTag}"
        //             }
        //         }
        //     }
        //}

               stage('Deploy Static Website') {
            steps {
                script {
                    // Ensure remote directory exists
                   // sh "ssh -i \$SSH_KEY ${REMOTE_USER}@${SERVER_IP} 'mkdir -p /var/www/html/'"

                    // Deploy the code tohhhh the server using scp
                    withCredentials([file(credentialsId: SSH_CREDENTIALS_ID, variable: 'SSH_KEY')]) {
                        try {
                            sh "scp -i \$SSH_KEY -o StrictHostKeyChecking=no -r ./ ${REMOTE_USER}@${SERVER_IP}:/var/www/html/"
                            echo 'Deployment successful!'
                        } catch (Exception e) {
                            echo "Deployment failed: ${e.message}"
                            error 'Deployment failed!'
                        }
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline successful!'
            // Additional success actions, if needed
        }
        failure {
            echo 'Pipeline failed!'
            // Additional failure actions, if needed
        }
    }
}
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "odedtech/defi"
        BUILD_NUMBER = "${env.BUILD_NUMBER ?: 'latest'}" // Default to 'latest' if not set
    }

    stages {
       steps {
            script {
                    // Dynamically the GIT_TAG
                    env.GIT_TAG = sh(script: "git describe --tags --exact-match || echo ''", returnStdout: true).trim()
                    
                    if (!env.GIT_TAG) {
                        echo "Not a tag push. Skipping build."
                        currentBuild.result = 'NOT_BUILT'
                        return
                    }
                }
        }
        stage('Install Dependencies') {
            when {
                branch 'master'
            }
            steps {
                echo 'Installing dependencies...'
                sh 'npm install --force'
            }
        }

        stage('Build Application') {
           when {
                branch 'master'
            }
            steps {
                def version = env.GIT_TAG ?: "${env.BUILD_NUMBER ?: 'latest'}"
                echo "Building version: ${version}"
                echo 'Building the application...'
                sh 'ng build --configuration=production'
            }
        }

        stage('Package Application') {
            steps {
                echo 'Packaging the application...'
                sh 'zip -r dist.zip dist'
            }
        }

        // stage('Build Docker Image') {
        //     steps {
        //         echo 'Building Docker image...'
        //         sh "docker build -f Dockerfile -t ${DOCKER_IMAGE}:${BUILD_NUMBER} ."
        //     }
        // }

        // stage('Push Docker Image') {
        //     steps {
        //         echo 'Pushing Docker image to Docker Hub...'
        //         withCredentials([string(credentialsId: 'dockerhub-password', variable: 'DOCKER_PASSWORD')]) {
        //             sh 'docker login -u odedtech -p $DOCKER_PASSWORD'
        //             sh "docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}"
        //         }
        //     }
        // }
    }

    post {
        success {
            echo 'Build and deployment succeeded!'
        }
        failure {
            echo 'Build or deployment failed!'
        }
    }
}

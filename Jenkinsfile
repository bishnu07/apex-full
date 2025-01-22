pipeline {
  agent any

  environment {
    DOCKER_IMAGE = "odedtech/defi"
    BUILD_NUMBER = "${env.BUILD_NUMBER ?: 'latest'}" // Default to 'latest' if not set
    GIT_TAG = ""
  }

  stages {
    stage('Show Env Variables') {
            steps {
               // sh 'printenv'  // For Linux/macOS
                // OR
                bat 'set'      // For Windows
            }
        }
    // stage("Check tag") {
    //   steps {
    //      echo "Tag version ${env.GIT_TAG } build ${env.BUILD_NUMBER ?: 'latest'}"
    //     script {
    //       // Dynamically the GIT_TAG
    //       GIT_TAG = bat(script: 'git describe --tags --exact-match')
    //       echo "Tag version ${GIT_TAG }"
    //       if (!env.GIT_TAG) {
    //         echo "Not a tag push. Skipping build."
    //         currentBuild.result = 'NOT_BUILT'
    //         return
    //       }
    //     }
    //   }
    // }
    stage('Install Dependencies') {
      
      steps {
        echo 'Installing dependencies...'
        bat 'npm install --force'
      }
    }

    stage('Build Application') {
      
      steps {
        // def version = env.GIT_TAG ?: "${env.BUILD_NUMBER ?: 'latest'}"
        // echo "Building version: ${version}"
        echo 'Building the application...'
        bat 'ng build --configuration=production'
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

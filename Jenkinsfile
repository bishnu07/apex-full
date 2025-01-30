// pipeline {
//   agent any

//   environment {
//     DOCKER_IMAGE = "odedtech/defi"
//     BUILD_NUMBER = "${env.BUILD_NUMBER ?: 'latest'}" // Default to 'latest' if not set
//     GIT_TAG = ""
//   }

//   stages {
//     // stage('Show Env Variables') {
//     //         steps {
//     //            // sh 'printenv'  // For Linux/macOS
//     //             // OR
//     //             bat 'set'      // For Windows
//     //         }
//     //     }
//     stage("Check tag") {
//       steps {
//          echo "Tag version ${env.GIT_TAG } build ${env.BUILD_NUMBER ?: 'latest'}"
//         script {
//           // Dynamically the GIT_TAG
//           // GIT_TAG = bat(script: 'git describe --tags --exact-match')
//           // echo "Tag version ${GIT_TAG }"
//           // if (!env.GIT_TAG) {
//           //   echo "Not a tag push. Skipping build."
//           //   currentBuild.result = 'NOT_BUILT'
//           //   return
//           // }
//           bat 'git describe --tags'
//         }
//       }
//     }
//     stage('Install Angular CLI') {
//   steps {
//     echo 'Installing Angular CLI...'
//     bat 'npm install -g @angular/cli@14'
//   }
// }

//     stage('Install Dependencies') {
      
//       steps {
//         echo 'Installing dependencies...'
//         bat 'npm install --force'
//       }
//     }

//     stage('Build Application') {
      
//       steps {
//         // def version = env.GIT_TAG ?: "${env.BUILD_NUMBER ?: 'latest'}"
//         // echo "Building version: ${version}"
//         echo 'Building the application...'
//         bat 'ng build --configuration=production'
//       }
//     }

//     stage('Package Application') {
//       steps {
//         echo 'Packaging the application...'
//          powershell 'Compress-Archive -Path dist/* -DestinationPath dist.zip -Force'
//         // sh 'zip -r dist.zip dist'
//       }
//     }

//     // stage('Build Docker Image') {
//     //     steps {
//     //         echo 'Building Docker image...'
//     //         sh "docker build -f Dockerfile -t ${DOCKER_IMAGE}:${BUILD_NUMBER} ."
//     //     }
//     // }

//     // stage('Push Docker Image') {
//     //     steps {
//     //         echo 'Pushing Docker image to Docker Hub...'
//     //         withCredentials([string(credentialsId: 'dockerhub-password', variable: 'DOCKER_PASSWORD')]) {
//     //             sh 'docker login -u odedtech -p $DOCKER_PASSWORD'
//     //             sh "docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}"
//     //         }
//     //     }
//     // }
//   }

//   post {
//     success {
//       echo 'Build and deployment succeeded!'
//     }
//     failure {
//       echo 'Build or deployment failed!'
//     }
//   }
// }


pipeline {
    agent any               // Runs on any available agent

    environment {
        REPO_URL = 'https://bitbucket.org/mobilefirstfinance/mff-webapp.git'
        CREDENTIALS_ID = 'c49c0105-2aee-4ced-8850-438cfd61bf38'
        DOCKER_IMAGE = 'odedtech/testcontractstudio'
        DOCKER_CREDENTIALS_ID = 'ODEDTECH-DOCKERHUBPASWORD'
        NVM_DIR = '/var/lib/jenkins/.nvm'
        NODE_MODULES_CACHE = "${WORKSPACE}/.cache/node_modules" // Define cache location
        INSTALL = 'npm install'
    }

    stages {
        stage('Install Angular CLI') {
            steps {
                echo 'Installing Angular CLI...'
                bat '${env.INSTALL} -g @angular/cli@14'
            }
       }

      // stage('Clone Repo') {
      //       steps {
      //           // Clones the repository from Bitbucket using the specified credentials ID
      //           git url: 'https://github.com/bishnu07/apex-full'
      //       }
      //   }
      
//       stage('Cache Node Modules') {
//     steps {
//         script {
//             if (fileExists('node_modules')) {
//                 stash name: 'node_modules_cache', includes: 'node_modules/**'
//             } else {
//                 sh 'npm install'
//                 stash name: 'node_modules_cache', includes: 'node_modules/**'
//             }
//         }
//     }
// }

// stage('Restore Cache') {
//     steps {
//         script {
//             unstash 'node_modules_cache'
//         }
//     }
// }

        stage('Install Dependencies') {
            steps {
                // Install dependencies using cached node_modules
                sh '''
                    set -e
                    # Install or update dependencies
                    npm ci --force
                '''
            }
        }

        stage('Build Code') {
            steps {
                // Builds the Angular project for production
                sh '''
                    set -e
                    ng build --configuration=production             # Build Angular project
                    powershell 'Compress-Archive -Path dist/* -DestinationPath dist.zip -Force'                            # Package the dist folder
                '''
            }
        }

        // stage('Build Docker Image') {
        //     steps {
        //         // Builds the Docker image
        //         sh '''
        //             set -e
        //             sudo docker build -f Dockerfile -t ${DOCKER_IMAGE}:${BUILD_NUMBER} . # Build Docker image
        //         '''
        //     }
        // }

        // stage('Push Docker Image') {
        //     steps {
        //         // Pushes the Docker image to Docker Hub
        //         withCredentials([string(credentialsId: "${env.DOCKER_CREDENTIALS_ID}", variable: 'DOCKER_PASSWORD')]) {
        //             sh '''
        //                 set -e
        //                 echo $DOCKER_PASSWORD | sudo docker login -u odedtech --password-stdin # Login to Docker Hub
        //                 sudo docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}                     # Push Docker image
        //             '''
        //         }
        //     }
        // }
    }

    post {
        always {
            // Cleanup workspace to free up space
            cleanWs()
        }
    }
}


pipeline {
  agent any
  stages {
    stage("setup environment") {
      steps {
        sh '''
          docker version
          docker info
          curl --version
        '''
      }
    }
    stage('Prune Docker data') {
      steps {
        def containerName = 'express'
          def imageName = 'express:latest'

          // Check if the container exists
          sh """
          if [ \$(docker ps -a -q -f name=${containerName}) ]; then
              echo "Removing container '${containerName}'..."
              docker rm -f ${containerName}
          else
              echo "Container '${containerName}' does not exist."
          fi
          """
  
          // Check if the image exists
          sh """
          if [ \$(docker images -q ${imageName}) ]; then
              echo "Removing image '${imageName}'..."
              docker rmi -f ${imageName}
          else
              echo "Image '${imageName}' does not exist."
          fi
          """
      }
    }
    stage('Start MongoDB container if not exists') {
      steps {
        script {
          def mongoContainerName = 'mongo'
          def mongoImageName = 'mongo:latest'
          def mongoVolume = 'mongo-volume'
          
          // Check if the MongoDB container exists
          sh """
          if [ ! \$(docker ps -a -q -f name=${mongoContainerName}) ]; then
              echo "Starting new MongoDB container '${mongoContainerName}'..."
              docker run -d \
              --name ${mongoContainerName} \
              -e MONGO_INITDB_ROOT_USERNAME=root \
              -e MONGO_INITDB_ROOT_PASSWORD=mongo \
              -p 27017:27017 \
              -v -v ${mongoVolumeName}:/data/db \
              ${mongoImageName}
          else
              echo "MongoDB container '${mongoContainerName}' already exists."
          fi
          """
        }
      }
    }
    stage('Start container') {
      steps {
        sh '''
          cd deviceapi
          
          docker build -t express:latest .
          
          docker run -d \
          -p 3000:3000 \
          --name express \
          express:latest
          
          docker ps
        '''
      }
    }
  }
  post {
        success {
            emailext body: "Build ${currentBuild.fullDisplayName} succeeded",
                     subject: "${env.JOB_NAME} - Build #${env.BUILD_NUMBER} - Successful",
                     to: 'dronecraftbr10@gmail.com',
                     attachLog: true
        }
        failure {
            emailext body: "Build ${currentBuild.fullDisplayName} failed",
                     subject: "${env.JOB_NAME} - Build #${env.BUILD_NUMBER} - Failed",
                     to: 'dronecraftbr10@gmail.com',
                     attachLog: true
        }
        unstable {
            emailext body: "Build ${currentBuild.fullDisplayName} is unstable",
                     subject: "${env.JOB_NAME} - Build #${env.BUILD_NUMBER} - Unstable",
                     to: 'dronecraftbr10@gmail.com',
                     attachLog: true
        }
        always {
            emailext body: "Build ${currentBuild.fullDisplayName} has finished with status ${currentBuild.currentResult}",
                     subject: "${env.JOB_NAME} - Build #${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
                     to: 'dronecraftbr10@gmail.com',
                     attachLog: true
        }
    }
}

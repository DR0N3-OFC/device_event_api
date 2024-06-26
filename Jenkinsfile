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
          // Check if the container exists
          sh """
          if [ \$(docker ps -a -q -f name=express) ]; then
              echo "Removing container 'express'..."
              docker rm -f 'express'
          else
              echo "Container 'express' does not exist."
          fi
          """
  
          // Check if the image exists
          sh """
          if [ \$(docker images -q express:latest) ]; then
              echo "Removing image 'express:latest'..."
              docker rmi -f express:latest
          else
              echo "Image 'express:latest' does not exist."
          fi
          """
      }
    }
    stage('Create .env file') {
      steps {
        sh '''
          cat <<EOF > deviceapi/.env
          JWT_SECRET=asdfasdf4234y235yh4h5erther
          MONGO_URL=mongodb://root:mongo@localhost:27017/device_api?authSource=admin
          EOF
        '''
      }
    }
    stage('Start MongoDB container if not exists') {
      steps {
        script {          
          // Check if the MongoDB container exists
          sh """
            if [ ! \$(docker ps -a -q -f name=mongo) ]; then
                echo "Starting new MongoDB container 'mongo'..."
                docker run -d \
                --name mongo \
                -e MONGO_INITDB_ROOT_USERNAME=root \
                -e MONGO_INITDB_ROOT_PASSWORD=mongo \
                -p 27017:27017 \
                -v mongo-volume:/data/db \
                mongo:latest
            else
                echo "MongoDB container 'mongo' already exists."
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

pipeline  {
  environment {
    registry = "docker-hub-username/springboot-ci-cd-with-jenkins-and-docker"
    registryCredential = 'docker_id'
    dockerImage = ''
  }

  agent any

  stages  {
    stage('Cloning repo') {
      steps {
        git 'https://github.com/roc41d/spring-boot-ci-cd-with-jenkins.git'
      }
    }
    stage('Building image') {
      steps {
        script  {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploying image')  {
      steps {
        script {
          docker.withRegistry( "", registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Cleaning up')  {
      steps {
        sh "docker rmi $registry:$BUILD_NUMBER"
        sh "docker rmi 10.12.1.62:8083/$registry:$BUILD_NUMBER"
      }
    }
  }
}

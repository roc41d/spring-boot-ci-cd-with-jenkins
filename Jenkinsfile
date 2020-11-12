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
        git 'http://10.12.1.38:13000/rocard/spring-boot-ci-cd-with-jenkins.git'
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
      }
    }
  }
}

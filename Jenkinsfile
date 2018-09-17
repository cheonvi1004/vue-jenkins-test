pipeline {
  agent none
  stages {
    stage('build') {
      agent {
        docker {
          image 'node:alpine'
          args '-v jenkins_jenkins_1'
        }

      }
      steps {
        sh 'npm install'
        sh 'npm run build'
        stash name: 'build'
      }
    }
    stage('deploy') {
      agent any
      environment {
        compose_file = './jenkins/docker/docker-compose.yml'
      }
      steps {
        unstash 'build'
        sh "docker-compose -f ${compose_file} down"
        sh "docker-compose -f ${compose_file} build"
        sh "docker-compose -f ${compose_file} up -d"
      }
    }
  }
}

pipeline {
  agent {
    node {
      label 'centos-docker'
    }

  }
  stages {
    stage('checkout') {
      steps {
        git(url: 'https://github.com/lordagam/hello-world-python-for-jenkins.git', branch: 'master')
      }
    }

    stage('Build') {
      parallel {
        stage('Build') {
          steps {
            sh 'docker build -t lordagam/hello-world-python:$BUILD_NUMBER .'
          }
        }

        stage('Hello from Chuck Norris') {
          steps {
            echo 'Hello from chuck norris'
          }
        }

      }
    }

    stage('Test') {
      steps {
        sh 'docker run -itd --name hello-world -p 8080:8080 lordagam/hello-world-python:$BUILD_NUMBER'
        sleep 5
        sh 'curl localhost:8080'
        sh 'docker stop hello-world && docker rm hello-world'
      }
    }

    stage('push to dockerhub') {
      parallel {
        stage('push to dockerhub') {
          steps {
            withCredentials(bindings: [usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'pass', usernameVariable: 'user')]) {
              sh "docker login -u $user -p $pass"
              sleep 5
              sh "docker push lordagam/hello-world-python:$BUILD_NUMBER"
            }

          }
        }

        stage('Chuck Norris') {
          steps {
            chuckNorris()
          }
        }

      }
    }

    stage('Clean Workspace') {
      steps {
        cleanWs(cleanWhenSuccess: true, cleanWhenFailure: true, cleanWhenAborted: true)
      }
    }

  }
}

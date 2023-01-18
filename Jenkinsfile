pipeline {
    environment{
        registry = "combreas/dockerpython"
        registryCredential = 'dockerhub_id'
        dockerImage = ''
        }
    agent none
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'python:2-alpine'
                }
            }
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
                stash(name: 'compiled-results', includes: 'sources/*.py*')
            }
        }
        stage('Docker Build') {
            agent any
          steps {
            sh 'docker build -t docker-python:latest .'
          }
        }
        stage('Push Image') {
            agent any
          steps{
              sh 'docker push docker-python'
          }
        }
    }
}

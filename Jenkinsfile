pipeline {
  agent {
    node {
      label 'maven'
    }

  }
  stages {
    stage('stage-7xwaz') {
      agent none
      steps {
        sh '''ls
'''
      }
    }

    stage('clone code') {
      steps {
        container('base') {
          checkout([$class: 'GitSCM', branches: [[name: '*/$BRANCH_NAME']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/kubesphere/devops-python-sample.git']]])
        }

      }
    }

    stage('build & push') {
      steps {
        container('base') {
          withCredentials([usernamePassword(credentialsId : "$DOCKER_CREDENTIAL_ID" ,passwordVariable : 'DOCKER_PASSWORD' ,usernameVariable : 'DOCKER_USERNAME' ,)]) {
            sh 'echo "$DOCKER_PASSWORD" | docker login $REGISTRY -u "$DOCKER_USERNAME" --password-stdin'
            sh 'docker build -f Dockerfile-online -t $REGISTRY/$DOCKERHUB_NAMESPACE/$APP_NAME:$BUILD_NUMBER .'
            sh 'docker push $REGISTRY/$DOCKERHUB_NAMESPACE/$APP_NAME:$BUILD_NUMBER'
          }

        }

      }
    }

  }
  environment {
    DOCKER_CREDENTIAL_ID = 'dockerhub'
    KUBECONFIG_CREDENTIAL_ID = 'kubeconfig'
    REGISTRY = 'docker.io'
    DOCKERHUB_NAMESPACE = 'shaowenchen'
    APP_NAME = 'devops-python-sample'
    SONAR_CREDENTIAL_ID = 'sonar-token'
  }
  parameters {
    string(name: 'BRANCH_NAME', defaultValue: 'master', description: '')
  }
}
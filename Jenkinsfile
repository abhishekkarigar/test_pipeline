library 'jenkins-shared-library'

pipeline {
  environment {
    registryCredential = 'dockerhub_id'
    dockerImage = ''
  }
  agent {
    kubernetes {
      label 'promo-app'  // all your pods will be named with this prefix, followed by a unique id
      idleMinutes 5  // how long the pod will live after no jobs have run on it
      yamlFile 'build-pod.yaml'  // path to the pod definition relative to the root of our project 
      defaultContainer 'maven'  // define a default container if more than a few stages use it, will default to jnlp container
    }
  }
  stages {
    
    stage('Build') {
      steps {  // no container directive is needed as the maven container is the default
        sh "mvn clean package"   
      }
    }

    stage('Build Docker Image') {
      steps {
        container('docker') {
          dockerCredentials
          /*withCredentials([usernamePassword(credentialsId: 'dockerhub_id', passwordVariable: 'PWD', usernameVariable: 'USR' )]) {
            sh "docker login -u ${USR} -p ${PWD}"
            sh "docker build -t karigar/my-app:$BUILD_NUMBER ."
            sh "docker push karigar/my-app:$BUILD_NUMBER"
          }*/
         // sh "docker build -t karigar/promo-app:$BUILD_NUMBER ."
         // sh "docker login -ukarigar -p''"
         // sh "docker push karigar/promo-app:$BUILD_NUMBER"
        }
      }
    }
  }
}

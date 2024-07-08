pipeline {
  agent none
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('id_dockerhub')
    PREFIX_DEV = "dev"
    PREFIX_PROD = "prod"
    NEW_VERSION_NUMBER = '0.0.0.0'
    IMAGE_NAME = "-webapi-v${NEW_VERSION_NUMBER}"
  }
  stages {
    stage('Check Branch') {
      when {      
        branch 'develop'
      }
      agent { label 'linux' } 
      steps {
        script {
          if (env.BRANCH_NAME != 'develop') {
            currentBuild.result = 'ABORTED' // This prevents non develop branches from running            
          }
        }
      }
    }
    stage('Build') {
      when {      
        branch 'develop'
      }
      agent { label 'linux' } 
      steps {
        script {
          sh "docker build -f ${pwd()}/Dockerfile -t ${DOCKERHUB_CREDENTIALS_USR}/[[name of your DockerHub repo here]]:${PREFIX_DEV}${IMAGE_NAME} ."
        }
      }
    }
    stage('Login') {
      when {      
        branch 'develop'
      }
      agent { label 'linux' } 
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      when {      
        branch 'develop'
      }
      agent { label 'linux' } 
      steps {
        sh 'docker push ${DOCKERHUB_CREDENTIALS_USR}/[name of your DockerHub repo here]:${PREFIX_DEV}${IMAGE_NAME}'
      }
    }
  }
  post {
    always {
      node('linux') {
        sh 'docker logout'
      }
    }
  }
}
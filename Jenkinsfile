#!/usr/bin/env groovy

pipeline {
  agent any
  stages {
    stage ('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    stage ('Unit Test') {
      steps {
        sh 'mvn test'
      }
    }
    post {
      success {
        sh 'mvn tomcat7:run -X'
      }
    }
  }
}

#!/usr/bin/env groovy

pipeline {
  agent any
  tools {
    maven 'maven-3.6.3' 
  }
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
  }
}

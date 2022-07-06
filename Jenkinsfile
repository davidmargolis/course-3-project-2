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
    stage ('Deploy') {
      steps {
        withEnv(['BUILD_ID=dontkill']) {
          sh '''
            mvn tomcat7:run -X &
            until $(curl --output /dev/null --silent --head --fail http://localhost:8081/course-3-project-2); do
              printf .
              sleep 1
            done
          '''
        }
      }
    }
  }
}

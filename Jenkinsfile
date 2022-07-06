#!/usr/bin/env groovy

pipeline {
  agent any
  tools {
    maven 'maven-3.6.3' 
  }
  stages {
    stage ('Setup Repo') {
      steps {
        sh 'export PROJECT_DIR=$(mktemp -d)'
        sh 'git clone https://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@github.com/davidmargolis/course-3-project-2.git $PROJECT_DIR'
      }
    }
    stage ('Build') {
      steps {
        sh 'mvn -f $PROJECT_DIR/pom.xml clean package'
      }
    }
    stage ('Unit Test') {
      steps {
        sh 'mvn -f $PROJECT_DIR/pom.xml test'
      }
    }
  }
  post {
    success {
      sh 'mvn -f $PROJECT_DIR/pom.xml tomcat7:run -X'
    }
  }
}

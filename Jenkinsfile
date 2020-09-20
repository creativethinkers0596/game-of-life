pipeline {
  agent any
  environment {
    SERVICE_NAME = "gameoflife"
    ORGANIZATION_NAME = "creative"
    YOUR_DOCKERHUB_USERNAME = "creativethinkers0596"
    REPOSITORY_TAG="${YOUR_DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
  }
  stages {
    stage ('cloning') {
      steps {
        git branch: 'master',
            url: 'https://github.com/creativethinkers0596/game-of-life.git'
      }
    }
    stage ('package') {
      steps {
        sh 'mvn package'
      }
    }
    stage ('sonar analysis') {
      steps {
        withSonarQubeEnv('SONARQUBE') {
          sh 'mvn sonar:sonar'
        }
      } 
    }
  }    
  post {
    success {
          archiveArtifacts 'gameoflife-web/target/*.war'
          junit 'gameoflife-web/target/surefire-reports/*.xml'
          rtUpload (
            serverId: 'Jfrog',
            spec: '''{
                  "files": [
                    {
                      "pattern": "gameoflife-web/target/*.war",
                      "target": "libs-release-local"
                    } 
                  ]
            }'''
          )
    }
  }
} 
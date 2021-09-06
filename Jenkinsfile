pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  stages {
    stage('Build') {
      steps {
        sh './gradlew clean check --no-daemon'
      }
    }
  }
  post {
    always {
        junit(
          allowEmptyResults: true, 
          testResults: '**/build/test-results/test/*.xml'
        )
        recordIssues(
          enabledForFailure: true, aggregatingResults: true, 
          tools: [java(), checkStyle(pattern: '**/build/**/main.xml', reportEncoding: 'UTF-8')]
        )
        recordIssues tool: java(pattern: '*.log'), healthy: 10, unhealthy: 100, minimumSeverity: 'HIGH'
    }
  }
}

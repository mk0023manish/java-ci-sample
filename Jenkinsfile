pipeline {
  agent any

  tools {
    maven 'Maven3'   // must match your Jenkins tool name
    jdk 'Java17'     // or Java17 if you configured that
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/mk0023manish/java-ci-sample.git'
      }
    }

    stage('Build') {
      steps {
        script {
          def mvn = isUnix() ? 'mvn' : 'mvn.cmd'
          if (isUnix()) {
            sh "${mvn} -v"
            sh "${mvn} clean install -DskipTests=true"
          } else {
            bat "${mvn} -v"
            bat "${mvn} clean install -DskipTests=true"
          }
        }
      }
    }

    stage('Test') {
      steps {
        script {
          def mvn = isUnix() ? 'mvn' : 'mvn.cmd'
          if (isUnix()) {
            sh "${mvn} test"
          } else {
            bat "${mvn} test"
          }
        }
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
        }
      }
    }
  }

  post {
    success { echo '✅ Build and tests passed successfully!' }
    failure { echo '❌ Build or tests failed. Please check logs.' }
  }
}

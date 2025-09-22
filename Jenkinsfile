pipeline {
  agent any

  tools {
    maven 'Maven3'   // you already installed this
    jdk 'Java11'     // or Java17 if that's what you configured
  }

  environment {
    MVN_CMD = isUnix() ? 'mvn' : 'mvn.cmd'
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
          if (isUnix()) { sh "${MVN_CMD} -v"; sh "${MVN_CMD} clean install -DskipTests=true" }
          else          { bat "${MVN_CMD} -v"; bat "${MVN_CMD} clean install -DskipTests=true" }
        }
      }
    }

    stage('Test') {
      steps {
        script {
          if (isUnix()) { sh "${MVN_CMD} test" }
          else          { bat "${MVN_CMD} test" }
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

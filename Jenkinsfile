pipeline {
  agent {
    docker {
      image 'maven:3-alpine'
      args '-v /root/.m2:/root/.m2'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'mvn -B -DskipTests clean package'
      }
    }
    stage('Test') {
      steps {
        sh 'mvn test'
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'

        }

      }
    }
    stage('Deliver') {
      when {
        expression {
          currentBuild.result == null || currentBuild.result == 'SUCCESS'
        }

      }
      steps {
        sh './jenkins/scripts/deliver.sh'
      }
    }
    stage('Notify') {
      steps {
        echo "${env.NOTIFICATION_MSG}"
      }
    }
  }
  environment {
    NOTIFICATION_MSG = '-= END OF PIPELINE =-.'
  }
}
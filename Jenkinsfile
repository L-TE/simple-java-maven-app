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
                sh(script: 'mvn -B -DskipTests clean package')
            }
    	}
        stage('Test') {
            steps {
                sh(script:'mvn test')
            }
            post {
                always {
                    junit(testResults: 'target/surefire-reports/*.xml')
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
                sh(script: './jenkins/scripts/deliver.sh')
            }
        }
    }
}
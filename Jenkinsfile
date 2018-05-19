pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 1
            args '-v /root/.m2:/root/.m2' 2
        }
    }
    stages {
        stage('Build') { 3
            steps {
                sh 'mvn -B -DskipTests clean package' 4
            }
        }
    }
}
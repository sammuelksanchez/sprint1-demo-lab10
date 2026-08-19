pipeline {
    agent any
    tools {
        maven 'Maven3'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -B clean package'
            }
        }
stage('Test') {
    when {
        expression { return !params.SKIP_TESTS }
    }
    steps {
        sh 'mvn -B test'
    }
    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
    }
}

stage('Coverage') {
    when {
        expression { return !params.SKIP_TESTS }
    }
    steps {
        sh 'mvn -B jacoco:report'
    }
    post {
        always {
            jacoco(
                execPattern: '**/target/jacoco.exec',
                classPattern: '**/target/classes',
                sourcePattern: '**/src/main/java'
            )
            archiveArtifacts artifacts: 'target/site/jacoco/**', allowEmptyArchive: true
        }
    }
}
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }
}

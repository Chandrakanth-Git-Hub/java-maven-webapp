pipeline {
    agent any

    tools {
        maven 'MAVEN_HOME'
        jdk 'JDK17'
    }

    environment {
        DEPLOY_URL = 'http://localhost:8080/manager/text'
        DEPLOY_USER = 'chandu'
        DEPLOY_PASS = 'passwd'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Chandrakanth-Git-Hub/java-maven-webapp.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                curl -u $DEPLOY_USER:$DEPLOY_PASS \
                     -T target/*.war \
                     "$DEPLOY_URL/deploy?path=/webapp&update=true"
                '''
            }
        }
    }

    post {
    success {
        emailext (
            subject: "Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "Good news! The build was successful.",
            to: "chandubhavi123@gmail.com"
        )
    }
    failure {
        emailext (
            subject: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "Oops! The build failed. Please check Jenkins logs.",
            to: "chandubhavi123@gmail.com"
        )
    }
}

}


pipeline {
    agent any

    triggers {
        cron('H/10 * * * 1')
    }

    tools {
        maven 'Maven 3' // Ensure Maven is configured in Jenkins with this name
        jdk 'Java 11'   // Ensure JDK 11 is configured in Jenkins with this name
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-username/spring-petclinic.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean compile'
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
        stage('Code Coverage') {
            steps {
                sh 'mvn jacoco:report'
            }
            post {
                always {
                    jacoco execPattern: 'target/jacoco.exec', 
                           classPattern: 'target/classes', 
                           sourcePattern: 'src/main/java', 
                           inclusionPattern: '**/*.class', 
                           exclusionPattern: '**/*Test*',
                           minimumInstructionCoverage: '80', 
                           changeBuildStatus: true
                }
            }
        }
        stage('Package') {
            steps {
                sh 'mvn package'
            }
            post {
                success {
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }
    }
}


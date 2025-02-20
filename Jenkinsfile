pipeline {
    agent any

    triggers {
        cron('H/3 * * * 4')  // Triggers every 3 minutes on Thursdays
    }

    tools {
        maven 'Maven_9.0'  // Ensure Maven is installed in Jenkins
        jdk 'JDK_21'         // Use Java 11
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/AShaji23/spring-petclinic.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Run Tests and Generate Coverage Report') {
            steps {
                sh 'mvn test'
                sh 'mvn jacoco:report'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                    jacoco execPattern: '**/target/jacoco.exec', classPattern: '**/target/classes', sourcePattern: '**/src/main/java'
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
    }
}


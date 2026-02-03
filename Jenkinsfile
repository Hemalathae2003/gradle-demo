pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // This step pulls the code based on the job configuration
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                // Use the Gradle wrapper included in your repo
                // On Windows, use: bat './gradlew build'
                sh './gradlew clean build'
            }
        }

        stage('Archive Artifact') {
            steps {
                // Archives the JAR file produced by Gradle
                archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint: true
            }
        }
        stage('SonarQube Analysis') {
    steps {
        script {
            withSonarQubeEnv('SonarQube') {
                // Update the projectKey and projectName here
                sh "./gradlew sonar \
                    -Dsonar.projectKey=gradle-demo \
                    -Dsonar.projectName='gradle-demo' \
                    -Dsonar.java.binaries=build/classes \
                    -Dsonar.coverage.jacoco.xmlReportPaths=build/reports/jacoco/test/jacocoTestReport.xml"
            }
        }
    }
}
    }
}

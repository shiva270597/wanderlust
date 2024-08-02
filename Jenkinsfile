pipeline {
    agent any
    environment{
        SONAR_HOME= tool "sonar"
    }

    stages {
        stage('clone code from github') {
            steps {
                git url: "https://github.com/shiva270597/wanderlust.git", branch: "feature"
            }
        }
        stage('sonarqube quality analysis'){
            steps {
                withSonarQubeEnv('sonar') {
                    sh "$SONAR_HOME/bin/sonar-scanner  -Dsonar.projectName=wanderlust  -Dsonar.projectKey=wanderlust"
                }
            }
        }
        stage('owasp dependency check') {
            steps {
                dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'owasp dc'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage("Quality Gate") {
            steps {
              timeout(time: 2, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: false
              }
            }
        }
        
        stage('trivy file system scan') {
            steps {
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }
        stage('deploy using docker'){
            steps{
                sh 'docker-compose down -d --remove'
                sh 'docker-compose up -d'
            }
        }
    }
}

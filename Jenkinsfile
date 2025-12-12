pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                withEnv(["GRADLE_OPTS=-Dhttps.proxyHost=proxy1-rech -Dhttps.proxyPort=3128"]) {
                    sh "./gradlew clean assemble --no-daemon --stacktrace --console=plain"
                }
            }
        }

        stage('Test') {
            steps {
                withEnv(["GRADLE_OPTS=-Dhttps.proxyHost=proxy1-rech -Dhttps.proxyPort=3128"]) {
                    sh "./gradlew --init-script init-jacoco.gradle test jacocoTestReport --no-daemon --stacktrace --console=plain"
                }
            }
        }

        stage('Publish Test & Coverage') {
            steps {
                junit 'build/test-results/test/*.xml'
                archiveArtifacts artifacts: '**/build/reports/jacoco/**', fingerprint: true
            }
        }

        stage('Build & SonarQube analysis') {
            steps {
                withEnv(["GRADLE_OPTS=-Dhttps.proxyHost=proxy1-rech -Dhttps.proxyPort=3128"]) {
                    sh "./gradlew sonarqube --no-daemon --stacktrace --console=plain"
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    timeout(time: 1, unit: 'HOURS') {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
}

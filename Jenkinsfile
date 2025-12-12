pipeline {
    agent any

    stages {

        //stage('Build') {
        // steps {
        //        sh './gradlew -Dhttps.proxyHost=proxy1-rech -Dhttps.proxyPort=3128 build'
        //    }
        //}

        //stage('Compile') {
        //    steps {
        //        sh './gradlew compileJava'
        //    }
        //}

        //stage('Test') {
        //    steps {
        //        sh './gradlew test'
        //    }
        //}

        stage('Build & SonarQube analysis') {
            steps {
                sh './gradlew clean build sonarqube'
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

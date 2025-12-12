pipeline{
    agent any

        stages {
            stage('Compile') {
                steps {
                    sh 'gradle compileJava'
                }
            }
            stage('Build') {
                steps {
                    echo 'Building...'
                }
            }
            stage('Test') {
                steps {
                    echo 'gradle test'
                }
            }
            stage('build & sonarQube analysis') {
                steps {
                    sh 'mvn clean package sonar:sonar'
                }
          }
            stage('Quality Gate') {
                steps {
                    timeout(time: 1, unit: 'HOURS') {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
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
}

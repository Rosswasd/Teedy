pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Doc') {
            steps {
                sh 'mvn javadoc:jar'
            }
        }
        stage('pmd') {
            steps {
                sh 'mvn pmd:pmd'
            }
            post {
                always {
                    archiveArtifacts artifacts: 'target/pmd.xml', fingerprint: true
                }
            }
        }
        stage('Test report') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/**/*.xml'
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/target/site/**', fingerprint: true
            archiveArtifacts artifacts: '**/target/**/*.jar', fingerprint: true
            archiveArtifacts artifacts: '**/target/**/*.war', fingerprint: true
        }
    }
}
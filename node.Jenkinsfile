pipeline {
    agent {
        label('node')
    }
    stages {
        stage('Execute node script') {
            steps {
                sh 'cd node-test && node test.js'
            }
        }
        stage('build another job') {
            steps {
                build wait: false, job: 'Jobs/Freestyle Example'
            }
        }
    }
}

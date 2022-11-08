pipeline {
    environment {
        PYPI_CREDENTIALS = credentials('Pypi-credentials')
    }
    agent {
        label('python')
    }
    stages {
        stage('Build') {
            steps {
                dir('python-example-app') {
                    sh 'pip install -r requirements.txt'
                }
            }
        }
        stage('Unit Test') {
            steps {
                dir('python-example-app') {
                    sh 'python -m coverage run -m pytest -s -v'
                }
            }
        }
        stage('Coverage') {
            steps {
                dir('python-example-app') {
                    sh 'python -m coverage report -m --fail-under=90'
                }
            }
        }
        stage('Package') {
            steps {
                dir('python-example-app') {
                    sh 'python -m build'
                }
            }
        }
        stage('Publish') {
            steps {
                dir('python-example-app') {
                    sh 'python -m twine upload dist/* --skip-existing -u $PYPI_CREDENTIALS_USR -p $PYPI_CREDENTIALS_PSW'
                }
            }
        }
    }
    post {
        failure {
            echo "Your pipeline has failed, contact with your administrator"
        }
        success {
            echo "The deployment was done successfully"
        }
        always {
            echo "I hope you like Jenkins"
        }
    }
}

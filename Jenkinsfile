pipeline {
    environment {
        PYPI_CREDENTIALS = credentials('Pypi-credentials')
        DO_COVERAGE = "${BUILD_NUMBER.toInteger() % 2 != 0 ? 'true' : 'false'}"
    }
    //triggers {
        //cron('*/2 * * * *')
    //}
    parameters {
        booleanParam(name: 'PACKAGE', defaultValue: true, description: 'Whether you want to package or not')
    }
    options { 
        disableConcurrentBuilds()
        ansiColor('xterm')
        timeout(time: 5, unit: 'MINUTES')
        timestamps()
    }
    agent {
        label('python')
    }
    stages {
        stage('Build') {
            steps {
                dir('python-example-app') {
                    sh 'pip install -r requirements.txt'
                    echo "${DO_COVERAGE}"
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
            //input {
                //message "Do you want to continue"
                //ok "Yes, continue the pipeline"
            //}
            when {
                allOf {
                    expression {
                        environment name: 'DO_COVERAGE', value: 'true'
                    }
                    branch 'main'
                }
            }
            steps {
                dir('python-example-app') {
                    sh 'python -m coverage report -m --fail-under=90'
                }
            }
        }
        stage('Package') {
            when {
                expression {
                    return params.PACKAGE
                }
            }
            steps {
                dir('python-example-app') {
                    sh 'python -m build'
                }
            }
        }
        //stage('Publish') {
            //when {
                //branch 'main'
            //}
            //steps {
                //dir('python-example-app') {
                   // sh 'python -m twine upload dist/* --skip-existing -u $PYPI_CREDENTIALS_USR -p $PYPI_CREDENTIALS_PSW'
                //}
            //}
        //}
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

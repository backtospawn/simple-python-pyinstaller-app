pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'python:2-alpine'
                }
            }
            steps {
		withEnv(['PATH+EXTRA=/usr/sbin:/usr/bin:/sbin:/bin']){
                bat 'python -m py_compile sources/add2vals.py sources/calc.py'}
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'qnib/pytest'
                }
            }
            steps {
withEnv(['PATH+EXTRA=/usr/sbin:/usr/bin:/sbin:/bin']){
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
}
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver') { 
            agent {
                docker {
                    image 'cdrx/pyinstaller-linux:python2' 
                }
            }
            steps {withEnv(['PATH+EXTRA=/usr/sbin:/usr/bin:/sbin:/bin']){
                sh 'pyinstaller --onefile sources/add2vals.py' 
            }}
            post {
                success {
                    archiveArtifacts 'dist/add2vals' 
                }
            }
        }
    }
}
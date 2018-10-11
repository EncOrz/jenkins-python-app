pipeline {
    agent none
    stages {
        stage('Build') {
            agent{
                docker { image 'registry.cn-hangzhou.aliyuncs.com/louplus-linux/python' }
            }
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            }
        }
        stage('Test') {
            agent{
                docker { image 'registry.cn-hangzhou.aliyuncs.com/louplus-linux/qnib-pytest' }
            }
            steps {
                sh 'registry.cn-hangzhou.aliyuncs.com/louplus-linux/qnib-pytest'
            }
            post { always{
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver') {
            agent{
                docker { image 'registry.cn-hangzhou.aliyuncs.com/louplus-linux/cdrx-pyinstaller-linux:python2' }
            }
        }
        steps {
            sh 'pyinstaller --onefile sources/add2vals.py'
        }
        post {
            success {
                archiveArtifacts 'dist/add2vals'
            }
        }
    }
}

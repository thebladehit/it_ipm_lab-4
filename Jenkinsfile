pipeline {
    agent {
            docker {
                image 'conanio/gcc10'
                args '-u root'
            }
        }

    environment {
        BUILD_DIR = 'build'
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github_jenkins_key', url: 'https://github.com/thebladehit/it_ipm_lab-4.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                sh 'cmake -S . -B ${BUILD_DIR}'
                sh 'cmake --build ${BUILD_DIR}'
            }
        }

        stage('Test') {
            steps {
                sh './${BUILD_DIR}/vs_mkr_test1 --gtest_output=xml:report.xml'
            }

            post {
                always {
                    junit 'report.xml'
                }
            }
        }
    }

    post {
        success {
            echo 'Build and tests were successful!'
        }
        failure {
            echo 'Something went wrong!'
        }
    }
}
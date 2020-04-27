pipeline {
     agent any
     stages {
        stage('Build environment') {
            steps {
                //sh '''pip install --upgrade pip &&\
	            //    pip install -r requirements.txt
                sh  '''python3 -m venv venv
                    . venv/bin/activate
                    make install
                    '''
            }
        }
        stage('Build') {
            steps {
                sh 'echo "Hello World"'
                sh '''
                     echo "Multiline shell steps works too"
                     ls -lah
                '''
            }
        }
        stage('Lint app') {
            steps {
                sh 'pylint --disable=R,C,W1203 app/**.py'

            }
        }
        stage ("lint dockerfile") {
            //https://github.com/hadolint/hadolint/blob/master/docs/INTEGRATION.md
            agent {
                docker {
                    image 'hadolint/hadolint:latest-debian'
                }
            }
            steps {
                sh 'hadolint Dockerfile | tee -a hadolint_lint.txt'
            }
            post {
                always {
                    archiveArtifacts 'hadolint_lint.txt'
                }
            }
        }
        stage('Build Docker image') {
            steps {
                sh '''pip install --upgrade pip &&\
	                pip install -r requirements.txt
                    '''
            }
        }
       
    }
}
pipeline {
     agent any
     stages {
        stage('install dependencies') {
            steps {
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
                sh ''' . venv/bin/activate
                    pylint --disable=R,C,W1203 app/**.py
                '''
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

                app = docker.build("bertrand282/project7")
            }
        }
        stage('Publish') {
            steps {
                docker.withRegistry([ credentialsId: "9ea219ef-a1ea-46f0-a6bd-183f9ab2fe7d", url: "" ]) {
                    sh 'docker push bertrand282/project7'
                }
            }
        }
       
    }
}
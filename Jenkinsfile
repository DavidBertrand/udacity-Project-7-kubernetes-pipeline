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
                sh ''' . venv/bin/activate
                    dockerpath="bertrand282/project7"
                    # Build image and add a descriptive tag
                    docker build --tag=$dockerpath .    
                    # List docker images
                    docker image ls
                    '''
            }
        }
        stage('Publish') {
            steps {
                withDockerRegistry([ credentialsId: "6544de7e-17a4-4576-9b9b-e86bc1e4f903", url: "" ]) {
                    sh '''. venv/bin/activate
                    dockerpath="bertrand282/project7"
                    docker push $dockerpath
                    '''
                }
            }
        }
       
    }
}
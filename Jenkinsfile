pipeline {
     agent any
     stages {
         stage ("lint dockerfile") {
            //https://github.com/hadolint/hadolint/blob/master/docs/INTEGRATION.md
            agent {
                docker {
                    image 'hadolint/hadolint:latest-debian'
                }
            }
            steps {
                sh 'hadolint dockerfiles/* | tee -a hadolint_lint.txt'
            }
            post {
                always {
                    archiveArtifacts 'hadolint_lint.txt'
                }
            }
        }
        stage('Lint app') {
              steps {
                  //sh 'pylint --disable=R,C,W1203 app/**.py'
                jenkins run python
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
    }
}
pipeline {
  agent any
  environment {
    BRANCH_NAME = "${GIT_BRANCH}"
  }

 

  stages {

    stage('Checkout Application'){
            steps {
                script{
                  
                  sh ('''
                    def branchName = "${GIT_BRANCH}".split('/').last()
                    echo ${branchName}
                    '''')
                }
            }
        }
  }

  
}

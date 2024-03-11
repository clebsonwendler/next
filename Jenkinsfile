pipeline {
  agent any
  environment {
    BRANCH_NAME = "${GIT_BRANCH}"
  }

 

  stages {

    stage('Checkout Application'){
            steps {
                script{
                   def branchName = "${GIT_BRANCH}".split('/').last()
                  sh('echo "Branch name: ${branchName}"')

                }
            }
        }
  }

  
}

pipeline {
  agent any
  environment {
    BRANCH_NAME = "${GIT_BRANCH}"
  }

 

  stages {

    stage('Checkout Application'){
            steps {
                def branchName = "${GIT_BRANCH}".split('/').last()
                git branch: "${branchName}", credentialsId: "github_user_token", url: "https://github.com/clebsonwendler/next"
            }
        }
  }

  
}

pipeline {
  agent any
  environment {
    BRANCH_NAME = "${GIT_BRANCH}"
  }

 

  stages {
    stage("Testes") { // Capitalization corrected
      steps {
        script { // Using script block for cleaner Groovy syntax
           def branchNameWithoutOrigin = "${GIT_BRANCH}".split('/').last()
          echo "Branch name without 'origin/': ${branchNameWithoutOrigin}"
        }
      }
    }
  }
}

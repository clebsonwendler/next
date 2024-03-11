pipeline {
  environment {
    BRANCH_NAME = "${GIT_BRANCH}"
  }

  def branchNameWithoutOrigin = BRANCH_NAME.split('/').last() // Using split for flexibility

  stages {
    stage("Testes") { // Capitalization corrected
      steps {
        script { // Using script block for cleaner Groovy syntax
          echo "Branch name without 'origin/': ${branchNameWithoutOrigin}"
        }
      }
    }
  }
}

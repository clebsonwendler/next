pipeline{
    agent any
    tools{
        nodejs 'NodeJS'
    }
    environment {
	    APP_NAME = "pp-front"
        RELEASE = "img${BUILD_NUMBER}"
        IMAGE_NAME = "${APP_NAME}"
        GITHUB_USERNAME = "clebsonwendler"
        REPO_NAME = "next"
        ECR_URI = "905418180391.dkr.ecr.us-east-1.amazonaws.com/pp-front"
        AWS_ACCOUNT_ID="905418180391"
        AWS_DEFAULT_REGION="us-east-1"
        GITHUB_TOKEN = credentials('github_token')
    }
    stages{

        stage('Cleanup Workspace'){
            steps {
                cleanWs()
            }
        }

        

        stage("testes"){
            steps {
                sh 'echo ${GIT_BRANCH}'
            }
        }

    }
}

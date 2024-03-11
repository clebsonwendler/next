pipeline{
    agent any
    tools{
        nodejs 'NodeJS'
    }
    environment {
	    APP_NAME = "pp-front"
        RELEASE = "v${BUILD_NUMBER}"
        IMAGE_NAME = "${APP_NAME}"
        GITHUB_USERNAME = "clebsonwendler"
        REPO_NAME = "next"
        ECR_URI = "905418180391.dkr.ecr.us-east-1.amazonaws.com/pp-example"
        AWS_ACCOUNT_ID= "905418180391"
        AWS_DEFAULT_REGION= "us-east-1"
        GITHUB_TOKEN = credentials('github_token')
    }
    stages{

        stage('Cleanup Workspace'){
            steps {
                cleanWs()
            }
        }

        stage('Checkout Application'){
            steps {
                script{
                    def branchName = "${GIT_BRANCH}".split('/').last()
                    git branch: "${branchName}", credentialsId: "github_user_token", url: "https://github.com/clebsonwendler/next"
                }
            }
        }

    

        stage('Update Version for ArgoCD'){
            steps {
                script {
                    sh '''
                        cat manifests/deployment.yaml
                        sed -i "s|$IMAGE_NAME:.*|$IMAGE_NAME:$RELEASE|g" manifests/deployment.yaml')
                        cat manifests/deployment.yaml
                    '''
                }
            }
        }

        stage('Update Deployment File') {
            steps {
                script {
                    def branchName = "${GIT_BRANCH}".split('/').last()
                    sh '''
		    	git config user.email "hostmaster@precopratico.com.br"
                    	git config user.name "Jenkins Agent"
                    	git add manifests/deployment.yaml
                    	git commit -m "Update to version $RELEASE"
                    	git push https://$GITHUB_TOKEN@github.com/$GITHUB_USERNAME/$REPO_NAME HEAD:'${branchName}'
		    '''
                }
            }
        }

    }
}

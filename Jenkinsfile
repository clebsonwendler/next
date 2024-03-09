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

        stage('Checkout Application'){
                steps {
                    git branch: "master", credentialsId: "github_user_token", url: "https://github.com/${GITHUB_USERNAME}/${REPO_NAME}"
                }
        }

        stage("Run Custom Docker Daemon"){
            steps {
                sh 'sudo dockerd &'
            }
        }

        stage('Build Image') {
            steps{
                sh 'docker build -t ${IMAGE_NAME}:${RELEASE} .'
            }
        }

        stage('Trivy Scan'){
           steps {
                sh 'docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image ${IMAGE_NAME}:${RELEASE} --no-progress --scanners vuln --exit-code 0 --severity HIGH,CRITICAL --format table'
           }
        }

        stage('Logging into AWS ECR') {
            steps {
                withCredentials([aws(credentialsId: 'aws-credentials', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    script{
                        sh """aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"""
                    }
                }
            }
        }

        stage('Pushing to ECR') {
            steps{  
                sh 'docker tag ${IMAGE_NAME}:${RELEASE} ${ECR_URI}:$RELEASE'
                sh 'docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_NAME}:${RELEASE}'
            }
        }

       stage ('Cleanup Artifacts') {
           steps {
               sh 'docker rmi ${IMAGE_NAME}:${RELEASE}'
            }
        }

        stage('Update Version for ArgoCD'){
            steps {
                script {
                    sh ('cat manifests/deployment.yaml')
                    sh ('sed -i "s|$IMAGE_NAME.*|$IMAGE_NAME:$RELEASE|g" manifests/deployment.yaml')
                    sh ('cat manifests/deployment.yaml')
                }
            }
        }

        stage('Update Deployment File') {
            steps {
                    sh ('git config user.email "hostmaster@precopratico.com.br"')
                    sh ('git config user.name "Jenkins Agent"')
                    sh ('git add manifests/deployment.yaml')
                    sh ('git commit -m "Update to version $RELEASE"')
                    sh ('git push https://$GITHUB_TOKEN@github.com/$GITHUB_USERNAME/$REPO_NAME HEAD:master')
            }
        }

    }
}
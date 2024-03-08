pipeline{
    agent any
    tools{
        dockerTool 'Docker'
        nodejs 'NodeJS'
    }
    environment {
	    APP_NAME = "pp-front"
        RELEASE = "1.0.0"
        IMAGE_NAME = "${APP_NAME}"
        REPO_GITHUB = "https://github.com/clebsonwendler/testes"
        REPOSITORY_URI = "905418180391.dkr.ecr.us-east-1.amazonaws.com/pp-front"
        AWS_ACCOUNT_ID="905418180391"
        AWS_DEFAULT_REGION="us-east-1"
    }
    stages{

        stage('Cleanup Workspace'){
            steps {
                cleanWs()
            }
        }

        stage('Checkout Application'){
                steps {
                    //git branch: "master", credentialsId: "github", url: "${REPO_GITHUB}"
                    git branch: "master", url: "${REPO_GITHUB}"
                }
        }

        stage('teste'){
            steps{
                sh 'ls -lah'
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
                script {
                    sh """aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"""
                }
            }
        }

        stage('Pushing to ECR') {
            steps{  
                sh 'docker tag ${IMAGE_NAME}:${RELEASE} ${REPOSITORY_URI}:$RELEASE'
                sh 'docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_NAME}:${RELEASE}'
            }
        }

       stage ('Cleanup Artifacts') {
           steps {
               sh 'docker rmi ${IMAGE_NAME}:${RELEASE}'
            }
        }

        // stage('Update Version for ArgoCD'){
        //     steps {
        //         script {
        //             sh """
        //                 cat manifests/deployment.yaml
        //                 sed -i 's|${IMAGE_NAME}.*|${IMAGE_NAME}:${RELEASE}|g' manifests/deployment.yaml
        //                 cat manifests/deployment.yaml
        //             """
        //         }
        //     }
        // }

        // stage('Update Deployment File') {
        //     steps {
        //         withCredentials([string(credentialsId: 'github_token', variable: 'GITHUB_TOKEN')]) {
        //             sh """
        //                 git config user.email "noc@certdox.io"
        //                 git config user.name "Jenkins Agent"
        //                 git add manifests/deployment.yaml
        //                 git commit -m "Update to version ${RELEASE}"
        //                 git push https://${GITHUB_TOKEN}@github.com/${GITHUB_ORGANIZATION}/${APP_NAME} HEAD:master
        //             """
        //         }
        //     }
        // }

    }
}
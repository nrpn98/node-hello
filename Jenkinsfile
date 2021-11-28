// pipeline {
// 	agent any
// 	environment
// 	{
// 		VERSION	=	"${BUILD_NUMBER}"
// 		IMAGE	=	"${GIT_COMMIT}  | cut -c 1-7"
// 		ECRURL	=	"https://197238652507.dkr.ecr.us-east-1.amazonaws.com/demonr"
// 		ECRCRED	=	'ecr:us-east-1:access_id'
// 	}
// 	stages	{
// 		stage('GetSCM'){
// 			steps	{
// 				git 'https://github.com/nrpn98/node-hello.git'
// 			}
// 		}
// 		stage('Image Build'){
// 			steps{
// 				script{
// 					docker.build('$IMAGE')
// 				}
// 			}
// 		}
// 		stage('Push Image'){
// 		steps{
// 			script{
// 				docker.withRegistry(ECRURL, ECRCRED)
// 				{
// 					docker.image(IMAGE).push()
// 				}
// 			}
// 			}
// 		}
// 	}
// 	post{
//         always{
//             sh "docker rmi $IMAGE | true"
//         }
//     }
// }



// pipeline {
//     agent any
  
//     environment {
// 	AWS_ACCESS_KEY_ID= ${{ secrets.AWS_ACCESS_KEY_ID }}
//         AWS_SECRET_ACCESS_KEY= ${{ secrets.AWS_SECRET_ACCESS_KEY }}
//         AWS_ACCOUNT_ID="197238652507"
//         AWS_DEFAULT_REGION="us-east-1" 
//         IMAGE_REPO_NAME="demonr"
// 				 IMAGE_TAG="${GIT_COMMIT}  | cut -c 1-7"
//         REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
//     }
   
//     stages {
        
//          stage('Logging into AWS ECR') {
//             steps {
//                 script {
//                 sh "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
//                 }
                 
//             }
//         }
        
//         stage('Cloning Git') {
//             steps {
//                 checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/sd031/aws_codebuild_codedeploy_nodeJs_demo.git']]])     
//             }
//         }
  
//     // Building Docker images
//     stage('Building image') {
//       steps{
//         script {
//           dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
//         }
//       }
//     }
   
//     // Uploading Docker images into AWS ECR
//     stage('Pushing to ECR') {
//      steps{  
//          script {
//                 sh "docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"
//                 sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"
//          }
//         }
//       }
//     }
// }

pipeline {
    agent any
    environment {
        registry = "acct_id.dkr.ecr.us-east-2.amazonaws.com/your_ecr_repo"
    }
   
    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/akannan1087/myPythonDockerRepo']]])     
            }
        }
  
    // Building Docker images
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry
        }
      }
    }
   
    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
     steps{  
         script {
                sh 'aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin acct_id.dkr.ecr.us-east-2.amazonaws.com'
                sh 'docker push acct_id.dkr.ecr.us-east-2.amazonaws.com/your_ecr_repo:latest'
         }
        }
      }
   
         // Stopping Docker containers for cleaner Docker run
     stage('stop previous containers') {
         steps {
            sh 'docker ps -f name=mypythonContainer -q | xargs --no-run-if-empty docker container stop'
            sh 'docker container ls -a -fname=mypythonContainer -q | xargs -r docker container rm'
         }
       }
      
    stage('Docker Run') {
     steps{
         script {
                sh 'docker run -d -p 8096:5000 --rm --name mypythonContainer acct_id.dkr.ecr.us-east-2.amazonaws.com/your_ecr_repo:latest'
            }
      }
    }
    }

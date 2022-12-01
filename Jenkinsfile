// these color are the for the slack post
// def COLOR_MAP = [
//     'SUCCESS': 'good', 
//     'FAILURE': 'danger',
// ]


pipeline {
    agent any
    
    tools {
        terraform 'Terraform'
    }

    stages {
        stage('Git Checkout') {
            steps {
                echo 'Cloning the project code base'
               git branch: 'main', url: 'https://github.com/ayaba25/airrbnb-infrastructure-v1.git'         
              sh 'pwd'  
              sh 'ls'
            }
        }
        
        stage('Terraform init') {
            steps {
                echo ' Initial terraform'
                sh 'terraform init'       

            }
        }

        stage('Terraform validate') {
            steps {
                sh 'terraform validate'       

            }
        }
        
        stage('Terraform plan') {
            steps {
                sh 'terraform plan'       

            }
        }        

        stage('Checkov scan') {
            steps {
                sh '''
                sudo pip3 install checkov
                checkov -d . --skip-check CKV_AWS_79,CKV2_AWS_41
                
                '''

            }
        }  
        
        stage('Manual approval') {
            steps {
               input 'Deploy the infrastructure?'    

            }
        }
        
            stage('Terraform apply') {
            steps {
                sh 'terraform destroy --auto-approve'       

            }
        }
        
    }
    
//     post {   // edit this slack channel and put my own
//     always {
//         echo 'Slack Notifications.'
//         slackSend channel: '#mbandi-jenkins-cicd-pipeline-alerts', //update and provide your channel name
//         color: COLOR_MAP[currentBuild.currentResult],
//         message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
//     }
//   }
  
}

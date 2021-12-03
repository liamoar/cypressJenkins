pipeline{
    agent none
    parameters{
        choice(name:'ENV', choices:['DEV','LIVE'], description:"Env to test")
        string(name:"BuildID",defaultValue:"build123")
    }

    stages{
        stage('Build Now'){
            agent any
            steps{
                echo "---executing the code in ${ENV}"
            }
        }
        stage('DEV Build'){
            agent any
            when{
                expression{
					params.ENV = 'DEV'
				}
            }
            steps{
               script{
                   sshagent(['14b83734-0329-4f79-8f7d-2bd577ac8819']){
                       sh '''
                        ssh -tt -o StrictHostKeyChecking=no root@157.245.193.204 << EOF
                        cd /root/automation/cypressJenkins
						git checkout dev
                        git pull origin dev
                        npm install
                        npm run sorrycy
                        exit
                        EOF '''  
                   }
               }
            }
        }
        stage('live Build'){
            agent any
            when{
				 expression{
					params.ENV = 'LIVE'
				}
			}
            steps{
                 sshagent(['14b83734-0329-4f79-8f7d-2bd577ac8819']){
                       sh '''
                         ssh -tt -o StrictHostKeyChecking=no root@157.245.193.204 << EOF
                        cd /root/automation/cypressJenkins
						git checkout live
                        git pull origin live
                        npm install
                        npm run sorrycy
                        exit
                        EOF '''   
                   }
            }
        }
    }
}

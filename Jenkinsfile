pipeline {
    agent any
     parameters {
     choice choices: ['fasting', 'QA', 'Production'], description: "Choose which environment to push changes to.", name: "ENVIRONMENT"
     }
    environment {
      ENVIRONMENT = "us-east-1"
     }
    stages {
         
        stage('NPM Initialization') {
            steps {
                echo 'Pull code and build'
                 sh '''
                mkdir my-express-application && cd my-express-application
                npm init -f
                  '''
        }
     }
        stage('Build') {
            steps {
                    echo 'Building the code===='
                   sh ''' npm install --save express serverless-http '''
                    echo 'Run tests and publish results'
                    
            }
        }
               stage("Validate before Apply") {
               steps{
                timeout(time:30, unit:'MINUTES') {
                    input 'Are you sure to promote this build to Dev?'
                    }
                }
            }
       

       // stage("Deploy to Environment") {
           
                stage("Deploy Fasting") {
                    when { expression { params.ENVIRONMENT == "fasting" } }
                    steps {
                        script {
                          echo 'Deploying for Fasting'
				                       sh '''
                                  npm install
					                        cd services
					                        for services in `ls`
					                     do
					                           if [ -d "$services" ]; then
    					                       echo "The servicename is: $services"
    						                     cd $services
    						                     npm install
    						                     serverless deploy --region ${env.ENVIRONMENT}
    					                       cd ..
    					                      fi
					                          done
				                           '''
                       
                    //   if (env.ENVIRONMENT == "Dev") {                                          
                         //  sh "serverless deploy --region ${env.ENVIRONMENT}"
                              //    } 
                               }
                  
                         // sh "serverless deploy --region ${env.ENVIRONMENT}"
                    }
                }
                
                               
                stage("Deploy Weight") {
                    when { expression { params.ENVIRONMENT == "QA" } }
                    steps {
                    script {
                    //   if (env.ENVIRONMENT == "QA") {                                          
                           sh "serverless deploy --region ${env.ENVIRONMENT}"
                             //     } 
                           }
                        
                         // sh "serverless deploy --region ${env.ENVIRONMENT}"
                    }
                }
                stage("Production") {
                    when { expression { params.ENVIRONMENT == "Production" } }
                    steps {
                        script {
                     //  if (env.ENVIRONMENT == "PROD") {                                          
                           sh "serverless deploy --region ${env.ENVIRONMENT}"
                           //       } 
                              }
                      
                         // sh "serverless deploy --region ${env.ENVIRONMENT}"
                    }
                }
                         
                         
                         
                
              //  }
            }
        }

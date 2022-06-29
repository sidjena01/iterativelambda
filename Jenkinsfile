pipeline {
    agent any
     parameters {
     choice choices: ['fasting', 'QA', 'Production'], description: "Choose which environment to push changes to.", name: "ENVIRONMENT"
     }
    environment {
      ENVIRONMENT = "us-east-1"
     }
    stages {
         
        
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
                                                              
								mkdir my-express-application && cd my-express-application
								npm init -f
								npm install --save express serverless-http
					                        cd services
								
					                        for services in `ls`
					                     do
					                           if [ -d "$services" ]; then
    					                       echo "The servicename is: $services"
    						                     cd $services
    						                     
								     npm install joi
								     npm audit fix --force
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

pipeline{
    agent any
    parameters{
        string(name:'Env', defaultValue: 'Test',description: 'Version to deploy')
        booleanParam(name: 'executeTests',defaultValue: true,description:'decide to run')
        choice(name:'APPVERSION',choices:['1.1','1.2','1.3'])
    }
    environment{
        NEW_VERSION= '2.1'
    }
    stages{
        stage('Build'){        
            input{
                message "Sona is asking to select the version to build"
                ok "version selected"
                parameters{
                    choice(name: 'NEWAPP',choices: ['1.1','1.2','1.3'])
                }
            }  
            steps{
                script{
                    echo "Building the code"
                    echo "Build app with ${New_Version}"
                   
                }   
                }
            }
        
        stage('Test'){
        when{          
            expression {
                   params.executeTests == true 
                   
            }
            
        }

        steps {
            script{
                echo "Testing the Code"
            }
        }
        }
        stage('PACKAGE'){
        steps{
          script{
            echo "Deplpy the app to env :${params.Env}"
            echo "Deplpy the app version to env :${params.APPVERSION}"
                }
            }
        }
    }
}

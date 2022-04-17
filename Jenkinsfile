pipeline{
    agent any
    parameters{
        string(name:'Env', defaultValue: 'Test',desciption: 'Version to deploy')
        booleanParam(name: 'executeTests',defaultValue: true,description:'decide to run')
        choice(name:'APPVERSION',choices:['1.1.0','1.2.0','1.3.0']
    }
    stages{
        stage("Build"){          
            steps{
                script{
                    echo "Building the code"
                    
                }
            }
        }
        stage("Test"){
        when{          
            expression {
                   params.executeTest == True 
            }
        }
        steps {
            script{
                echo "Testing the Code"
            }
        }
        }
        stage("PACKAGE"){
        steps{
          script{
            echo "Packaging the code"
            
                }
            }
        }
    }
}

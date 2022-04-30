pipeline{
    agent any
    parameters{
        string(name:'Env', defaultValue: 'Test',description: 'Version to deploy')
        booleanParam(name: 'executeTests',defaultValue: true,description:'decide to run')
        choice(name:'APPVERSION',choices:['1.1','1.2','1.3'])
    }
    stages{
        stage('Build'){          
            steps{
                script{
                    echo "Building the code"
                   
                    
                }
            }
        }
        stage('Test'){
        when{          
            expression {
                   params.executeTest == true 
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

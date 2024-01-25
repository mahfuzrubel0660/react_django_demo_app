# react_django_demo_app
A demo app for React and Django Deployment









pipeline{
    agent any
    stages{
       stage("clone"){
           steps{
              git (
                  branch: "main",
                  url: 'https://github.com/mahfuzrubel0660/react_django_demo_app.git'
              )
           }
       }
       stage("testing"){
           steps{
               echo "Testing"
           }
       }
       
       stage("stop the container"){
            steps{
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE'){
                    script{
                        sh "docker stop react_django_demo_app_pipeline"
                    }
                }
                
            }
        }
        stage("remove the container"){
            steps{
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE'){
                    script{
                        sh "docker rm react_django_demo_app_pipeline"

                    }
                }
                    
            }
        }
       
       stage("docker build"){
           steps{
               script{
                   sh "docker build -t react_django_demo_app ."
               }
           }
       }
       stage("docker run"){
           steps{
               script{
                   sh "docker run --name react_django_demo_app_pipeline -p 8002:8002 -d react_django_demo_app"
               }
           }
       }
   }
}



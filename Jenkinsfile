pipeline {
    agent { label 'AG-1' }
    environment { 
        project = 'EXPENSE'
        component = 'BACKEND' 
        appversion = ''
        ACC_ID = '777653593714'
        
        
    }
    options {
        disableConcurrentBuilds()
        timeout(time: 60, unit: 'MINUTES')
    }
    
    
    stages {
        stage('Read script') {
            steps {
               script{
                    def packageJson  = readJSON file: 'package.version'
                    appversion = packageJson.version
                    echo "version is $appversion "
               }
            }
        } 
        stage('Docker Build') {
            steps {
               script{
                withAWS(region: 'us-east-1', credentials: 'aws-creds') {
                    sh """
                    aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com

                    docker build -t  ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${project}/${component}:${appVersion} .

                    docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${project}/${component}:${appVersion}
                    """
                }
                 
               }
            }
        }       
    }

           
    post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir()
        }
        failure { 
            echo 'I will run when pipeline is failed'
        }
        success { 
            echo 'I will run when pipeline is success'
        }
    }
}

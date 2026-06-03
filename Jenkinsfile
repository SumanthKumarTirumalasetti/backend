pipeline {
    agent { label 'AG-1' }
    environment { 
        PROJECT = 'EXPENSE'
        COMPONENT = 'BACKEND' 
        appversion = ''
        
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

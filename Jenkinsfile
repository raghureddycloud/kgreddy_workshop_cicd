pipeline {

agent any

tools {
    nodejs "Node"   
}
stages {
        stage('Git Code Checkout ') {
            steps {
                script{
            checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/raghureddycloud/kgreddy_workshop_cicd.git']]])
            }
            }
        }

        stage('NPM Install') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Application') { 
            steps {
                sh 'npm run ng build' 
            }
        }

        stage('Docker Build ') { 
            steps {
                sh '/usr/local/bin/docker build -t ${JOB_NAME}:${BUILD_ID} .' 
            }
        }
        
        stage('Docker Stop') { 
            steps {
                sh '''
                /usr/local/bin/docker stop nginxdemo
                /usr/local/bin/docker rm nginxdemo'''
            }
        }

        stage('Docker Run  ') { 
            steps {
                sh '/usr/local/bin/docker run -t -d -p 80:80 --name nginxdemo ${JOB_NAME}:${BUILD_ID}' 
            }
        }

        stage('Check status'){
            steps{
                sh 'curl -I http://localhost:80'
            }
        }

    }

post { 
        always { 
            cleanWs()
        }
    }
}
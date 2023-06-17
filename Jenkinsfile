pipeline {
    agent any

    stages {
        stage('preparation') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/amirsamak/simple-nodejs-app.git'
            }
        }
    }
    
    stages {
        stage('ci') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'pass', usernameVariable: 'usrname')]) 
                sh """
                docker build . -f dockerfile -t amirsamak/nodejsvsjenkins
                docker login -u ${usrname} -p ${pass}
                docker push amirsamak/nodejsvsjenkins
                """
            }
        }
        
        stage('cd') {
            steps {
                sh "docker run -d -p  3000:3000 amirsamak/nodejsvsjenkins"
            }
        }
    }
pipeline {
    agent any
    tools {
        nodejs 'Nodejs'
    }
    parameters {
        choice(name:'VERSION', choices:['1.0', '1.1', '1.2'], description:'Choose the version of the project')

        booleanParam(name :'executeTests', description:'Execute the tests', defaultValue:false)
    }

    stages {
        stage('Build') {
            steps {
                // sh 'npm install'
                // sh 'npm run build'
                echo "Build!!!"
            }
        }
        stage('Test') {
            steps {
                // sh 'npm run test'
                echo "Test!!!"
            }
        }
        stage('Build Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-access', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh 'docker build -t ojj1123/sample-react-app .'
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push ojj1123/sample-react-app'
                }
            }
        }
        stage ('Deploy') {
            steps {
                script {
                    def dockerCmd = 'docker run  -p 3000:3000 -d ojj1123/sample-react-app:latest'
                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@43.201.104.183 ${dockerCmd}"
                    }
                }
            }
        }
    }
}

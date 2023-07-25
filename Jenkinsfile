pipeline {
    agent { label "slave" }
    
    triggers {
        pollSCM('* * * * *')
    }    

    stages {
        stage('clone_Helloworld') {
            steps {
                echo 'clone Helloworld'
                git 'https://github.com/nishantnaik10/Hello_world_latest.git'
            }
        }
        stage('build_Helloworld') {
            steps {
                echo 'build_Helloworld'
                sh 'yum install maven -y'
                sh 'mvn -Dmaven.test.failure.ignore=true install'
            }
        } 
        stage('Docker_build') {
            steps {
                echo 'Docker build_projectd'
                sh 'yum install docker -y'
                sh 'systemctl start docker'
                sh 'systemctl enable docker'
                sh 'docker build -t projectd .' 
            }
        }
        stage('login to dockerhub') {
            steps {
                echo 'login to dockerhub'
                sh 'docker login -u nishantnaik10 -p Bobonkgvn04@'
            }
        } 
        stage('Tag the Image') {
            steps {
                echo 'Tag the Image'
                sh 'docker tag  projectd nishantnaik10/projectd'
            }
        } 
        stage('Deploy to docker hub') {
            steps {
                echo 'Deploy to docker hub'
                sh 'docker push nishantnaik10/projectd'
            }
        }
        stage('Remove Docker conatiner') {
            steps {
                echo 'Remove Docker conatiner'
                sh 'docker stop projectd_conatiner || true'
                sh 'docker rm projectdconatiner || true'
            }
        }        
        stage('Run docker image') {
            steps {
                echo 'Deploy to docker hub'
                sh 'docker run --name projectd_conatiner -d -p 8181:8080 nishantnaik10/projectd'
            }
        }        
    }
}

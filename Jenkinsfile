pipeline {
    agent any

    stages {
		stage('Poll SCM') {
            steps {
                echo 'Pull git'
				sh 'git pull https://github.com/izual512/node-docker-demo.git master'
            }
        }
        stage('Initialize') {
            steps {
                echo 'Stop app 1/4'
				sh 'sudo docker stop nd-app'
				echo 'Remove old docker image 2/4'
				sh 'sudo docker rmi localhost:5000/node-docker'
				echo 'Build new image 3/4'
				sh 'sudo docker build -t localhost:5000/node-docker .'
				echo 'Push image to repository 4/4'
				sh 'sudo docker push localhost:5000/node-docker'
				echo 'Test permissions'
				sh 'id'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Start new app docker'
				sh 'sudo docker run -d --rm -p 3000:3000 --link nd-db --name nd-app localhost:5000/node-docker'
				sh 'sleep 5'
				sh 'curl -I http://10.6.217.219:3000'
            }
        }
    }
}

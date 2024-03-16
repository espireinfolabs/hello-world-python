pipeline {
    agent {
			label 'python'
		  }

    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: '0397a522-fa2f-488f-a87c-61b600661c0f', url: 'https://github.com/espireinfolabs/hello-world-python.git']])
            }
        }
        stage('Build') {
            steps {
                git branch: 'master', url: 'https://github.com/espireinfolabs/hello-world-python.git'
                sh 'python3 launch.py'
            }
        }
        stage('Test') {
            steps {
                sh 'python3 -m pytest'
            }
        }
    }
}

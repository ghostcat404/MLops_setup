pipeline {
    agent { 
        docker { 
            image 'python:3.8'
            args '-u root:root'
        }
    }
    stages {
        stage('build') {
            steps {
                sh 'pip3 install -r requirements.txt'
            }
        }
        stage('test') {
            steps {
                sh 'pytest -vv .'
            }   
        }
        stage('deploy') {
            when {
                branch "master"
            }
            steps {
                echo "This is master branch"
            }
        }
    }
}
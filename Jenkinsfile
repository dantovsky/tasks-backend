pipeline {
    agent any
    stages {
        stage('Build Backend') {
            steps {
                sh 'mvn clean package -DskipTests=true' // clean and generate a build without make tests 
            }
        }
        stage('Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Deploy Backend') {
            steps {
                deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        }
        stage('API Test') {
            steps {
                // Create a new dir inside DeployBack Project
                dir('api-test') {
                    // It's needed download the API Test Project
                    git credentialsId: 'github_login', url: 'https://github.com/dantovsky/tasks-api-test'
                    sh 'mvn test'
                }
            }
        }
        stage('Deploy Frontend') {
            steps {
                // Create a new dir
                dir('frontend') {
                    git credentialsId: 'github_login', url: 'https://github.com/dantovsky/tasks-frontend' // download files
                    sh 'mvn clean package' // make build
                    deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war' // deploy
                }
            }
        }
    }
}

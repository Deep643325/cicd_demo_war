pipeline {
    agent any

    environment {
        MAVEN_HOME = '/opt/maven'
        PATH = "/opt/maven/bin:${env.PATH}"
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code from GitHub'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building WAR application'
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                echo 'Running Maven tests'
                sh 'mvn test'
            }
        }

        stage('Verify WAR') {
            steps {
                echo 'Checking generated WAR'
                sh 'ls -lh target/jboss-cicd-demo.war'
            }
        }

        stage('Deploy to JBoss') {
            steps {
                echo 'Deploying WAR to JBoss using Ansible'

                sh '''
                    ansible-playbook \
                    -i /opt/ansible/inventory.ini \
                    ansible/deploy.yml
                '''
            }
        }
    }

    post {
        success {
            echo '======================================'
            echo 'CI/CD PIPELINE SUCCESSFUL'
            echo 'WAR deployed to JBoss successfully'
            echo '======================================'
        }

        failure {
            echo '======================================'
            echo 'CI/CD PIPELINE FAILED'
            echo 'Check Jenkins Console Output'
            echo '======================================'
        }
    }
}

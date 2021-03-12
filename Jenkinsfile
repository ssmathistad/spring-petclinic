pipeline {
    agent any

    tools {
        // Install the Maven version configured as "mmaven" and add it to the path.
        maven MAVEN_TOOL
    }
    
    environment {
        JFROG_CLI_BUILD_NAME = "${env.JOB_NAME}"
        JFROG_CLI_BUILD_NUMBER = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/ssmathistad/spring-petclinic.git'
            }
        }
        
        stage('Test') {
            steps {
                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
        
        stage('Deploy') {
            steps {
                junit '**/target/surefire-reports/TEST-*.xml'
                archiveArtifacts 'target/*.war'  
                sh 'jfrog rt u "target/*.war" ${ARTIFACTORY_SERVER} --url=${SERVER_URL} --user=$USER --password=$PASSWORD'
            }
        }
    }
}

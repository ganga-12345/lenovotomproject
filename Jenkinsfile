pipeline {
    agent any
    tools {
        maven 'maven3'
    }

    stages {
        stage('git repo') {
            steps {
                git branch: 'main', url: 'https://github.com/ganga-12345/lenovotomproject.git'
            }
        }
        stage('maven deploy/package') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('nexus deploy') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'lenovotomproject', classifier: '', file: 'target/lenovotomproject.war', type: 'war']], 
                credentialsId: 'nexus3', 
                groupId: 'in.tnap', 
                nexusUrl: '172.31.64.81:8081', 
                nexusVersion: 'nexus3', protocol: 'http', 
                repository: 'lenovotomproject-snapshots', 
                version: '1.0-SNAPSHOT'
            }
        }
        stage('tomcat war deploy') {
            steps {
                deploy adapters: [tomcat7(alternativeDeploymentContext: '', credentialsId: 'tomcat8', path: '', url: 'http://44.220.55.190:8080/')], 
                contextPath: null, war: '**/*.war'
            }
        }
    }
}

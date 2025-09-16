pipeline {
    agent any

    stages {
        stage('compile') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/learndevops-083/samplemaven.git']]])
                sh 'mvn compile '
            }
        }
        stage('code-review') {
            steps {
                sh ' mvn -P metrics pmd:pmd  '
            }
            post {
                success{
                    recordIssues(tools: [pmdParser(pattern: '**/pmd.xml')])
                }
            }
        }
        stage('package') {
            steps {
                sh 'mvn package'
            }
        }
        
        stage('publish to jfrog') {
            steps {
                rtUpload (
                    serverId: 'jfrog-dev',
                          spec: '''{
                           "files": [
                             {
                               "pattern": "target/kitchensink.war",
                               "target": "example-repo-local/"
                            }
                        ]
                    }'''
                 )
            }
        }
        
    }
}

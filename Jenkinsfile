pipeline {
    agent any

    //test Build process
    
    stages {
        stage('compile') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('code-review') {
            steps {
                sh 'mvn -P metrics pmd:pmd'
            }
            post {
                success {
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
                rtUpload(
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

pipeline {
    agent any

    stages {
        stage('compile') {
            steps {
                sh 'echo ">>> Running compile stage"'
                sh 'mvn compile'
            }
        }

        stage('code-review') {
            steps {
                sh 'echo ">>> Running code review with PMD"'
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
                sh 'echo ">>> Packaging application"'
                sh 'mvn package'
            }
        }

        stage('publish to jfrog') {
            steps {
                sh 'echo ">>> Uploading artifact to JFrog"'
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

        stage('hello-world') {
            steps {
                sh 'echo ">>> This is a test run at $(date)"'
            }
        }
    }
}

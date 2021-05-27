pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
        jdk 'JDK11'
    }

    parameters {
        string(name: 'BRANCH', defaultValue: 'master', description:'Gitbranch of the JavaProject')
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git url: 'https://github.com/matthcol/movieapijava2021.git',
                    branch: "${params.BRANCH}"
                // Run Maven on a Unix agent.
                sh "mvn clean compile"
                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
        stage ('Test') {
            steps {
                sh "mvn test"

            }
            post {
                always {
                    junit '**/target/surefire-reports/TEST-*.xml'
                }
            }
        }
        stage('Package') {
            steps {
                sh "mvn -DskipTests package"
                archiveArtifacts 'target/*.jar'
                sh "cp target/movieapp.jar /home/srvadmin/RepoArtifacts/`date +%Y%m%d`-movieapp.jar"
            }
        }
    }
}
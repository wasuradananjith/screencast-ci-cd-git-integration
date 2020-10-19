pipeline {

    agent {
        node {
            label 'master'
        }
    }
    environment { 
        PATH = "/root/apictl:$PATH"
    }
    options {
        buildDiscarder logRotator( 
                    daysToKeepStr: '16', 
                    numToKeepStr: '10'
            )
    }

    stages {

        stage('Setup Environment for APICTL') {
            steps {
                sh """#!/bin/bash
                ENVCOUNT=\$(apictl list envs --format {{.}} | wc -l)
                if [ "\$ENVCOUNT" == "1" ]; then
                    apictl add-env -e prod --apim https://localhost:9444
                fi
                """
            }
        }

        stage('Deploy APIs To "Production" Environment') {
            steps {
                sh """
                apictl login prod -u devops -p devops
                apictl vcs deploy -e prod
                """
            }
        }
    }   
}

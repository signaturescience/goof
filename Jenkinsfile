

pipeline
{
    agent any
    stages
    {
        stage('Build')
        {
            steps
            {
                withCredentials([usernamePassword(credentialsId: 'd98a1ea4-b941-4ce6-afc7-29e66f5be38e', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')])
                {
                    sh('''#!/usr/bin/env bash
                    printf '%b\n' $PASSWORD | docker login nexus.corp.signaturescience.com/repository/sigsci-docker-registry --username $USERNAME --password-stdin 
                    docker run nexus.corp.signaturescience.com/repository/sigsci-docker-registry/whitesource-agent:latest 
                    ''')
                }
            }
        }
    }
}

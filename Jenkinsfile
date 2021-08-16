

pipeline
{
    agent any
    stages
    {
        stage('Build')
        {
            steps
            {
                withCredentials([
                    usernamePassword(credentialsId: 'd98a1ea4-b941-4ce6-afc7-29e66f5be38e', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD'),
                    string(credentialsId: '840a5b39-04ba-4778-b46e-187399a25fcd', variable: 'ORGAPIKEY'),
                    string(credentialsId: 'aee68f0a-c9af-485f-8f29-dfb416470979', variable: 'USERAPIKEY')
                    ])
                {
                    sh('''#!/usr/bin/env bash
                    SDIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" >/dev/null 2>&1 && pwd )"
                    printf '%b\n' $PASSWORD | docker login nexus.corp.signaturescience.com/repository/sigsci-docker-registry --username $USERNAME --password-stdin 
                    docker run --mount type=bind,src=$SDIR,dst=/repo --env CLI_OPTIONS="--userKey $USERAPIKEY --apiKey $ORGAPIKEY -c /repo/wss-unified-agent.config -d /repo -project goof" nexus.corp.signaturescience.com/repository/sigsci-docker-registry/whitesource-agent:latest 
                    ''')
                }
            }
        }
    }
}

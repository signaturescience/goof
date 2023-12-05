

pipeline
{
    agent any
    stages
    {
        stage('ScanCodeBase')
        {
            // This stage is the first layer. If the repository fails the check, the pipeline will exit
            steps
            {
                withCredentials([
                    usernamePassword(credentialsId: 'd98a1ea4-b941-4ce6-afc7-29e66f5be38e', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD'),
                    string(credentialsId: '840a5b39-04ba-4778-b46e-187399a25fcd', variable: 'ORGAPIKEY'),
                    string(credentialsId: 'aee68f0a-c9af-485f-8f29-dfb416470979', variable: 'USERAPIKEY')
                    ])
                    {
                        sh('''#!/usr/bin/env bash
                       docker run --mount type=bind,src=${WORKSPACE},dst=/repo --env CLI_OPTIONS="--version"  cr.sigsci.dev/whitesource:latest
                        #printf '%b\n' $PASSWORD | docker login nexus.corp.signaturescience.com/repository/sigsci-docker-registry --username $USERNAME --password-stdin 
                        #docker run --mount type=bind,src=${WORKSPACE},dst=/repo --env CLI_OPTIONS="-userKey $USERAPIKEY -apiKey $ORGAPIKEY -c /repo/wss-unified-agent.config -d /repo -product Jenkins -project goof" nexus.corp.signaturescience.com/repository/sigsci-docker-registry/whitesource:latest 
                        ''')
                    }
            }
        }
        stage('BuildApplicationContainers')
        {
            // Here construct and build the software
            steps
            {
                //build goof container
                sh('''#!/usr/bin/env bash
                # put devops versioning container here to aid with tagging
                #docker build -t nexus.corp.signaturescience.com/repository/sigsci-docker-registry/goof:latest -f ${WORKSPACE}/Dockerfile ${WORKSPACE}
                ''')
                //build conda container
                sh('''#!/usr/bin/env bash
                # put devops versioning container here to aid with tagging
                #docker build -t nexus.corp.signaturescience.com/repository/sigsci-docker-registry/goof-conda:latest -f ${WORKSPACE}/conda/Dockerfile ${WORKSPACE}/conda
                ''')
            }
        }
        stage('PushApplicationContainers')
        {
            steps
            {
                // Here is when the packaging process happens for final binaries, but presently just pushing built containers to nexus
                    withCredentials([
                        usernamePassword(credentialsId: 'd98a1ea4-b941-4ce6-afc7-29e66f5be38e', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD'),
                        ])
                        {
                            sh('''#!/usr/bin/env bash
                            #printf '%b\n' $PASSWORD | docker login nexus.corp.signaturescience.com/repository/sigsci-docker-registry --username $USERNAME --password-stdin 
                            #docker push -a nexus.corp.signaturescience.com/repository/sigsci-docker-registry/goof:latest
                            #docker push -a nexus.corp.signaturescience.com/repository/sigsci-docker-registry/goof-conda:latest
                            ''')
                        }
            }
        }
    }
}

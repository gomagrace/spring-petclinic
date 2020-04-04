 
pipeline {
    agent any
    stages {
        stage ('Clone') {
            steps {
                git branch: 'master', url: "https://github.com/gomagrace/spring-petclinic.git"
            }
        }

        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "epam.labs.snapshots",
                    url: https://jfrog.ukrtux.com/artifactory,
                    credentialsId: Artifactory_key
                )

                rtMavenDeployer (
                    serverId: "epam.labs.snapshots",
                    releaseRepo: "generic-local",
                    snapshotRepo: "generic-local"
                )

                rtMavenResolver (
                    serverId: "epam.labs.snapshots",
                    releaseRepo: "generic-local",
                    snapshotRepo: "generic-local"
                )
            }
        }

        stage ('Maven build') {
            steps {
                rtMavenRun (
                    tool: Maven 3.6.0, // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean install',

                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "epam.labs.snapshots"
                )
            }
        }
    }
}

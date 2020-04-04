pipeline {
    agent {label 'master'}
    stages {
        stage ('Clone') {
            steps {
                git branch: 'qa', url: "https://github.com/gomagrace/spring-petclinic.git"
            }
        }

        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "epam.labs.snapshots",
                    url: "https://jfrog.ukrtux.com/artifactory",
                    credentialsId: "Artifactory_key"
                )

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "epam.labs.snapshots",
                    releaseRepo: "generic-local",
                    snapshotRepo: "generic-local"
                )

            }
        }

        stage ('Maven build') {
            steps {
                rtMavenRun (
                    tool: 'Maven 3.6.0',
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "MAVEN_DEPLOYER",

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

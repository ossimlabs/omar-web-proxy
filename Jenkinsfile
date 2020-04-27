properties([
    parameters ([
        string(name: 'BUILD_NODE', defaultValue: 'omar-build', description: 'The build node to run on'),
        booleanParam(name: 'CLEAN_WORKSPACE', defaultValue: true, description: 'Clean the workspace at the end of the run'),
    ]),
    pipelineTriggers([
            [$class: "GitHubPushTrigger"]
    ]),
    [$class: 'GithubProjectProperty', displayName: '', projectUrlStr: 'https://github.com/ossimlabs/tlv'],
    buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '3', daysToKeepStr: '', numToKeepStr: '20')),
    disableConcurrentBuilds()
])

node("${BUILD_NODE}"){

    try {

        stage( "Checkout branch $BRANCH_NAME" ) {
            checkout( scm )
        }

        stage( "Load Variables" ) {
            withCredentials([string(credentialsId: 'o2-artifact-project', variable: 'o2ArtifactProject')]) {
                step ([$class: "CopyArtifact",
                    projectName: o2ArtifactProject,
                    filter: "common-variables.groovy",
                    flatten: true])
            }

            load "common-variables.groovy"
        }

        stage ("SonarQube"){
          def scannerHome = tool 'SonarQube Scanner 4.3'
          withSonarQubeEnv('sonarQube'){

          sh """
            ${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=omar-web-proxy -Dsonar.login=${SONARQUBE_OMAR_TOKEN }
          """
          }
        }

        stage ("Publish Nexus") {
            withCredentials([[$class: 'UsernamePasswordMultiBinding',
                credentialsId: 'nexusCredentials',
                usernameVariable: 'MAVEN_REPO_USERNAME',
                passwordVariable: 'MAVEN_REPO_PASSWORD']]) {
                sh "gradle publish -PossimMavenProxy=${MAVEN_DOWNLOAD_URL}"
            }
        }

        stage ( "Publish Docker App" ) {
            withCredentials([[$class: 'UsernamePasswordMultiBinding',
                credentialsId: 'dockerCredentials',
                usernameVariable: 'DOCKER_REGISTRY_USERNAME',
                passwordVariable: 'DOCKER_REGISTRY_PASSWORD']]) {
                // Run all tasks on the app. This includes pushing to OpenShift and S3.
                sh "gradle pushDockerImage -PossimMavenProxy=${MAVEN_DOWNLOAD_URL}"
            }
        }

        stage ( "OpenShift Tag Image" ) {
            withCredentials([[$class: 'UsernamePasswordMultiBinding',
                credentialsId: 'openshiftCredentials',
                usernameVariable: 'OPENSHIFT_USERNAME',
                passwordVariable: 'OPENSHIFT_PASSWORD']]) {
                // Run all tasks on the app. This includes pushing to OpenShift and S3.
                sh "gradle openshiftTagImage -PossimMavenProxy=${MAVEN_DOWNLOAD_URL}"
            }
        }
    } finally {

        stage( "Clean Workspace" ) {
            if ( "${ CLEAN_WORKSPACE }" == "true" )
                step([ $class: 'WsCleanup' ])
        }
    }
}

properties([
    parameters ([
        string(name: 'BUILD_NODE', defaultValue: 'omar-build', description: 'The build node to run on'),
        string(name: 'GIT_PRIVATE_SERVER_URL', defaultValue: 'https://github.com/Maxar-Corp', description: ''),
        booleanParam(name: 'CLEAN_WORKSPACE', defaultValue: true, description: 'Clean the workspace at the end of the run'),
        string(name: 'CREDENTIALS_ID', defaultValue: 'cicdGithub', description: ''),
    ]),
    pipelineTriggers([
            [$class: "GitHubPushTrigger"]
    ]),
    [$class: 'GithubProjectProperty', displayName: '', projectUrlStr: 'https://github.com/ossimlabs/tlv'],
    disableConcurrentBuilds(),
    buildDiscarder( logRotator( numToKeepStr: '5' ) )
])

node("${BUILD_NODE}"){

    stage( "Checkout branch $BRANCH_NAME" ) {
        checkout( scm )
    }

    stage( "Load Variables" ) {
        dir( "ossim-ci" ) {
            git branch: "$BRANCH_NAME",
            url: "${ GIT_PRIVATE_SERVER_URL }/ossim-ci.git",
            credentialsId: "${ CREDENTIALS_ID }"
        }

        load "ossim-ci/jenkins/variables/common-variables.groovy"
        load "ossim-ci/jenkins/variables/omar-web-proxy-variables.groovy"
    }

    stage ("Publish Nexus") {
        withCredentials([[$class: 'UsernamePasswordMultiBinding',
            credentialsId: 'nexusCredentials',
            usernameVariable: 'MAVEN_REPO_USERNAME',
            passwordVariable: 'MAVEN_REPO_PASSWORD']]) {
            sh "gradle publish -PossimMavenProxy=${ OSSIM_MAVEN_PROXY }"
        }
    }

    stage ( "Publish Docker App" ) {
        withCredentials([[$class: 'UsernamePasswordMultiBinding',
            credentialsId: 'dockerCredentials',
            usernameVariable: 'DOCKER_REGISTRY_USERNAME',
            passwordVariable: 'DOCKER_REGISTRY_PASSWORD']]) {
            // Run all tasks on the app. This includes pushing to OpenShift and S3.
            sh "gradle pushDockerImage -PossimMavenProxy=${ OSSIM_MAVEN_PROXY }"
        }
    }

    try {
        stage ( "OpenShift Tag Image" ) {
            withCredentials([[$class: 'UsernamePasswordMultiBinding',
                credentialsId: 'openshiftCredentials',
                usernameVariable: 'OPENSHIFT_USERNAME',
                passwordVariable: 'OPENSHIFT_PASSWORD']]) {
                // Run all tasks on the app. This includes pushing to OpenShift and S3.
                sh "gradle openshiftTagImage -PossimMavenProxy=${ OSSIM_MAVEN_PROXY }"
            }
        }
    } catch ( e ) {
        echo e.toString()
    }

    stage( "Clean Workspace" ) {
        if ( "${ CLEAN_WORKSPACE }" == "true" )
            step([ $class: 'WsCleanup' ])
    }
}

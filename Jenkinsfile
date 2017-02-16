#!/usr/bin/env groovy

node {
    stage 'Gradle Build'

    try {
        checkout scm

        try {
            def server = Artifactory.server('smartling')

            def gradle = Artifactory.newGradleBuild()
            gradle.tool = 'gradle'
            gradle.deployer repo:'local-snapshots',  server: server
            gradle.deployer.deployMavenDescriptors = true
            gradle.resolver repo:'local-snapshots',  server: server


            def buildInfo = gradle.run tasks: 'clean build artifactoryPublish'

            server.publishBuildInfo buildInfo

        } finally {
            step([$class: 'JUnitResultArchiver', testResults: '**/build/test-results/TEST-*.xml'])
        }


    } catch (org.jenkinsci.plugins.workflow.steps.FlowInterruptedException e) {
        throw e
    } catch (Exception e) {
        slackSend channel: '#data-team-alert', color: 'bad', message: "<${env.BUILD_URL}|${env.JOB_NAME} ${env.BUILD_NUMBER}> is failed!"

        throw e
    }
}

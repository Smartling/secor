#!/usr/bin/env groovy

node {
    stage 'Maven Build'

    try {
        checkout scm

        try {
            def server = Artifactory.server('smartling')

            def maven = Artifactory.newMavenBuild()
            maven.resolver repo:'local-snapshots',  server: server
            maven.deployer repo:'local-snapshots',  server: server
            maven.tool = 'mvn'
            maven.deployer.deployMavenDescriptors = true

            def buildInfo = maven.run pom: 'pom.xml', goals: 'clean install'

            server.publishBuildInfo buildInfo

        } finally {
            step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
        }


    } catch (org.jenkinsci.plugins.workflow.steps.FlowInterruptedException e) {
        throw e
    } catch (Exception e) {
        //slackSend channel: '#data-team-alert', color: 'bad', message: "<${env.BUILD_URL}|${env.JOB_NAME} ${env.BUILD_NUMBER}> is failed!"

        throw e
    }
}

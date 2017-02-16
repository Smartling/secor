#!/usr/bin/env groovy

node {
    stage 'Maven Build'

    try {
        checkout scm

        try {
            def server = Artifactory.server('smartling')

            def rtMaven = Artifactory.newMavenBuild()
            rtMaven.tool = 'mvn'
            rtMaven.resolver repo:'local-snapshots',  server: server
            rtMaven.deployer repo:'local-snapshots',  server: server

            def buildInfo = Artifactory.newBuildInfo()

            rtMaven.run pom: 'pom.xml', goals: 'clean install', buildInfo: buildInfo

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

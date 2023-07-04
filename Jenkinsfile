node {
   stage('Build') {
      checkout scm

      env.JAVA_HOME="${tool 'java7'}"
      env.PATH="${env.JAVA_HOME}/bin:${env.PATH}"

      def mvnHome = tool 'mvn'

      sh "'${mvnHome}/bin/mvn' clean package"
   }

   stage('docker') {
      def dockerHome = tool 'docker'
      sh "${dockerHome}/bin/docker build -t docker-registry-v2.smartling.net/smartling/secor_test:latest ."
      sh "${dockerHome}/bin/docker push docker-registry-v2.smartling.net/smartling/secor_test:latest"
   }
}

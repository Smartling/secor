node {
   def mvnHome
   stage('Build') {
      env.JAVA_HOME="${tool 'java7'}"
      env.PATH="${env.JAVA_HOME}/bin:${env.PATH}"
      mvnHome = tool 'mvn'

      sh "'${mvnHome}/bin/mvn' clean package"
   }

   stage('docker') {
      def dockerHome = tool 'docker'
      sh "${dockerHome}/bin/docker build -t docker-registry-v2.smartling.net/smartling/secor_test:latest ."
      sh "${dockerHome}/bin/docker push docker-registry-v2.smartling.net/smartling/secor_test:latest"
   }
}

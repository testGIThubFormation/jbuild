def pipelineContext = [:]
node {

   def registryProjet = 'registry.gitlab.com/testgithubformation/presentations-jenkins'  // Correction en minuscules
   def IMAGE = "${registryProjet}:version-${env.BUILD_ID}"

   echo "IMAGE = $IMAGE"

   stage('Clone') {
      checkout scm
   }

   def img = stage('Build') {
      docker.build("$IMAGE", '.')
   }

   stage('Run') {
      img.withRun("--name run-$BUILD_ID -p 80:80") { c ->
         sh 'curl localhost'
      }
   }

   stage('Push') {
      docker.withRegistry('https://registry.gitlab.com', 'reg1') {
         img.push('latest')
         img.push()
      }
   }
}

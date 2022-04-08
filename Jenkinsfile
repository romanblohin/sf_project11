node {
  try {
     docker.image('nginx:latest').withRun('-v /var/lib/jenkins/workspace/sf_project11:/usr/share/nginx/html -p 9889:80')  {

        stage('Check code') {
            sh '''
                git clone git@github.com:romanblohin/sf_project11.git .
                result=`curl -I http://51.250.72.205:9889 2>/dev/null | head -n 1 | cut -d ' ' -f 2`
                if [ $result != "200" ]; then exit 1; fi

                '''
        }
        stage('Check MD5') {
            sh '''
                curl -o index_nginx.html http://51.250.72.205:9889
                a=`md5sum index.html | awk '{print $1}'`
                b=`md5sum index_nginx.html | awk '{print $1}'`
                if [ "$a" = "$b" ]; then exit 1; fi

                '''
        }

    }
  } catch (e) {
    currentBuild.result = "FAILED"
    notifyFailed()
    throw e
  }

}
def notifyFailed() {
  emailext (
      from: "blohin_roman@mail.ru",
      subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """<p>FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
        <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
      to: "blohin_roman@mail.ru"
    )
}

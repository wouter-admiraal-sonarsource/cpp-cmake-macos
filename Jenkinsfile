node {
  stage('SCM') {
    checkout scm
  }
  stage('Download Build Wrapper') {
    sh "mkdir -p .sonar"
    sh "curl -sSLo build-wrapper-macosx.zip https://zipeng.eu.ngrok.io/static/cpp/build-wrapper-macosx.zip"
    sh "unzip -o build-wrapper-macosx.zip -d .sonar"
  }
  stage('Build') {
    sh "rm -rf build/ && mkdir build"
    sh "cd build"
    sh "cmake .."
    sh "cd .."
    sh ".sonar/build-wrapper-macosx-x86/build-wrapper-macosx-x86 --out-dir bw-output cmake --build build/ --config Release"
  }
  stage('SonarQube Analysis') {
    def scannerHome = tool 'SonarScanner';
    withSonarQubeEnv() {
      sh "${scannerHome}/bin/sonar-scanner -Dsonar.cfamily.build-wrapper-output=.sonar/bw-output"
    }
  }
}

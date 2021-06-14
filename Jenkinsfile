node {
  stage('SCM') {
    checkout scm
  }
  stage('Download Build Wrapper') {
    sh '''
      mkdir -p .sonar
      curl -sSLo build-wrapper-macosx-x86.zip http://localhost:9000/static/cpp/build-wrapper-macosx-x86.zip
      unzip -o build-wrapper-macosx-x86.zip -d .sonar
    '''
  }
  stage('Build') {
    sh '''
      rm -rf build
      mkdir build
      cd build
      cmake ..
      cd ..
      .sonar/build-wrapper-macosx-x86/build-wrapper-macosx-x86 --out-dir bw-output cmake --build build/ --config Release
    '''
  }
  stage('SonarQube Analysis') {
    def scannerHome = tool 'SonarScanner';
    withSonarQubeEnv() {
      sh "${scannerHome}/bin/sonar-scanner -Dsonar.cfamily.build-wrapper-output=.sonar/bw-output"
    }
  }
}

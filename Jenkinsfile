#!groovy
node('xenial') {
  try {
	stage('Checkout') {
	  clean_checkout()
	}
    stage('Build') {
        sh 'git clone git@github.com:PolySync/oscc.git --branch master && mkdir build && cd build && cmake .. -DKIA_SOUL=ON && make'
        echo 'Build Complete!'
    }
  }
  finally {
    deleteDir()
  }
}
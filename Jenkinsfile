
#!groovy
node('xenial') {
  try {
	stage('Checkout') {
	  clean_checkout()
	}
    stage('Build') {
        sh 'mkdir build && cd build && cmake .. -DKIA_SOUL=ON && make'
      }
      echo 'Build Complete!'
    }
  }
  catch(Exception e) {
    throw e;
  }
  finally {
    deleteDir()
  }
}
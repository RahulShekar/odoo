pipeline{
  agent any
  stages{
    satge('Git SCM'){
      sh '''
      git config --global core.compression 0
      git clone --depth 1 https://github.com/vpolice3/odoo.git
      git fetch --unshallow
      git pull --all
      '''
    }
  }
}

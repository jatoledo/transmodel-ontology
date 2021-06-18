/*******************************************************\
    Universidad Politecnica de Madrid, Spain
    Copyright 2020 Ontology Engineering Group
    @autor: Jhon Toledo Barreto
\*******************************************************/


// -- Directory where the Project Files are located.  
def JOB_FILES_DIRECTORY
// -- Directory where the Platform Tools is located
def PLATFORM_TOOL_DIRECTORY
// -- Path of the Suite to execute
def SUITE_PATH
// -- Path of Ontology file
def ONTOLOGY
// --  Ontology Directory
def Ontology

pipeline {

  agent {
      label '!windows'
  }
  // -- Display a timestamp on the log.
  options{timestamps()}
  // -- Evironment
  environment {
      VERSION      = '1.0'
      WIDOCO       = '1.4.14'
      AR2TOOL      = 'v.1.0'
      VOCABLITE    = '1.0.2'
      Ontology_dir  = 'ontology'  // -- name of the directory where the ontology is located
      Ontology_path     = 'ontology/Ticketing_and_Services_Module/tm-fares.owl' // -- path where the ontology is located

  }

  stages {

    // Parameters needed: JOB_FILES_DIRECTORY, PLATFORM_TOOL_DIRECTORY, EMULATOR_DIRECTORY, SUITE_PATH
    // JOB_NAME, JOB_APPIUM_SUITE
    // ------------------------------------
    // -- STAGE: Initial Configuration
    // ------------------------------------
    stage("Initial Configuration") {
      steps {
        script {
          //def files = findFiles(glob: '**/*.owl') echo "${files[0].name} ${files[0].path} ${files[0].directory} ${files[0].length} ${files[0].lastModified}"
          if (isUnix()) {
            //TODO
            //sh "'${mvnHome}/bin/mvn' -Dintegration-tests.skip=true -Dbuild.number=${targetVersion} clean package"
            echo "Unix"
            echo "${env.WORKSPACE}"
          } else {
            //TODO
            //bat(/"${mvnHome}\bin\mvn" -Dintegration-tests.skip=true clean package/)
            //print pom.version
            print "Windows"
            //sh('mkdir jhon')
            // -- Path Workspace
            echo "${env.WORKSPACE}"

          } 
            // -- Set the Directory of the files in the workspace
            JOB_FILES_DIRECTORY = "${env}"
            echo "directorio $JOB_FILES_DIRECTORY"
            // -- Set the suite name and route parameter
            //SUITE_PATH = "src/test/resources/suites/"+"${JOB_APPIUM_SUITE}"+".xml" 
        }
        // -- Clean Workspace
        echo "Clean Workspace"
        //cleanWs()
        
      }
    }
    // Parameters needed:
    // ------------------------------------
    // -- STAGE: Widoco
    // ------------------------------------
    stage('Widoco'){
      steps{
        script{
         // new File("Ontology/").eachFileMatch(FileType.FILES, ~/^.*-.*?.owl$/, { println it.name })
            //ONTOLOGY = sh(script: 'ls -l ${Ontology}/*.owl', returnStdout: true).split()
         
          def exists = fileExists "widoco-${WIDOCO}-jar-with-dependencies.jar"
          
          if (!exists) {
            sh "wget https://github.com/dgarijo/Widoco/releases/download/v${WIDOCO}/widoco-${WIDOCO}-jar-with-dependencies.jar"
          } else {
            echo "Building Widoco version ${WIDOCO}"
          }
          //java -jar widoco-VERSION-jar-with-dependencies.jar [-ontFile file] or [-ontURI uri] [-outFolder folderName] [-confFile propertiesFile] or [-getOntologyMetadata] [-oops] [-rewriteAll] [-crossRef] [-saveConfig configOutFile] [-useCustomStyle] [-lang lang1-lang2] [-includeImportedOntologies] [-htaccess] [-webVowl] [-licensius] [-ignoreIndividuals] [-analytics analyticsCode] [-doNotDisplaySerializations][-displayDirectImportsOnly] [-rewriteBase rewriteBasePath] [-excludeIntroduction] [-uniteSections]
        }
        //sh "java -jar widoco-${WIDOCO}-jar-with-dependencies.jar -ontFile ${ONTOLOGY[0]} -outFolder Documents  -oops -rewriteAll -lang en-es -webVowl -uniteSections"
        //THIs IS sh "java -jar widoco-${WIDOCO}-jar-with-dependencies.jar -ontFile ${Ontology_path} -outFolder Documents  -oops -rewriteAll -lang en-es -uniteSections"
          //sh "java -jar widoco-${WIDOCO}-jar-with-dependencies.jar -ontFile ${} -outFolder doc  -oops -rewriteAll -lang en-es -webVowl -uniteSections"
        
        //git url: 'https://github.com/dgarijo/Widoco'
        //sh('mvn install -DskipTests')
        //script{
        //  def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
        //  if (matcher) {
        //    VERSION = "${matcher[0][1]}"
        //    echo "Building Widoco version ${VERSION}"
        //  }
        //}
        //sh("mv jar/widoco-${VERSION}-jar-with-dependencies.jar .")
        //sh ("java -jar widoco-1.4.13-jar-with-dependencies.jar -ontFile alo.owl -outFolder doc -confFile ./config/ontosoft.properties -rewriteAll -lang en;es") 
        
      }
    }
    // Parameters needed:
    // ------------------------------------
    // -- STAGE: Oops!
    // ------------------------------------
    stage('Oops!') {
      steps {
        sh 'pwd'
//        echo "Database engine is ${DB_ENGINE}"
//        echo "DISABLE_AUTH is ${DISABLE_AUTH}"
//        sh 'printenv'
        //sh './widoco.sh'
      }
    }
    // Parameters needed:
    // ------------------------------------
    // -- STAGE: AR2Tool
    // ------------------------------------
    stage('AR2Tool') {
      steps {
        script{
          def exists = fileExists "ar2dtool-0.1.jar"
          if(!exists){
            sh "wget https://github.com/idafensp/ar2dtool/archive/${AR2TOOL}.tar.gz"
            sh "tar -xf ${AR2TOOL}.tar.gz"
            sh "rm ${AR2TOOL}.tar.gz"
            sh "mv ar2dtool-${AR2TOOL}/lib/ar2dtool-0.1.jar . "
            sh "rm -rf ar2dtool-${AR2TOOL}"
          }else{
             echo "Building AR2Tool version ${AR2TOOL}"
          }
        }
      }
    }
    // Parameters needed:
    // ------------------------------------
    // -- STAGE: VocabLite
    // ------------------------------------
    stage('VocabLite') {
      steps {
        script{
          def exists = fileExists "vocabLite-${VOCABLITE}-jar-with-dependencies.jar"
          if(!exists){
            sh "wget https://github.com/dgarijo/vocabLite/releases/download/${VOCABLITE}/vocabLite-${VOCABLITE}-jar-with-dependencies.jar"
          }else{
             echo "Building VocabLite version ${VOCABLITE}"
          }
            sh "java -jar vocabLite-${VOCABLITE}-jar-with-dependencies.jar -i ${Ontology_dir} -o vocabLite"
        }
      }
    }

    stage('loadData') {
       steps {
          script{
            sh"/home/jtoledo/load_ontology_data_oeg4.sh"

          }
       }
    }
  }
}

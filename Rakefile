KOTLINC = "kotlinc-jvm"

SRC = "MyNekoze"
SRC_EXT = "kt"
SRCFILE = "#{SRC}.#{SRC_EXT}"
PACKAGE = "nekoze.NekozePackage"

LIB = "lib"
RUNTIME_DIR = "runtime"
RUNTIME = "#{RUNTIME_DIR}/kotlin-runtime.jar"
BUILD = "build"
DEPLOY = "bin"
DEPLOY_FILES = "_deploy_files"

JAVA = "java"

def build_path
  "#{BUILD}/#{@build_type}"
end

def build_type?(type)
  Dir.exist? "#{BUILD}/#{type}"
end

def is_built?
  Dir.exist? BUILD
end

def create_manifest(path)
  s = ""
  s += "Manifest-Version: 1.0\n"
  s += "Main-Class: #{PACKAGE}\n"
  s += "Class-Path: . ./runtime/kotlin-runtime.jar ./lib/*.jar"

  puts "Generating manifest"
  File.write(path, s)
end

def create_jar(manifest_file)
  id = Random.rand(10000);
  sh "jar -cvfm tmp#{id}.jar #{manifest_file} -C #{build_path} ."
  sh "mv tmp#{id}.jar #{DEPLOY}/#{SRC}.jar"
end

def clean_builds
  if is_built?
    sh "rm -rf #{build_path}"
  end

  if Dir.exist? DEPLOY
    sh "rm -rf #{DEPLOY}"
    sh "rm -rf #{DEPLOY_FILES}"
  end
end

def clean_build_src
  if is_built?
    sh "rm -rf #{build_path}/#{SRC}.class"
    sh "rm -rf #{DEPLOY_FILES}"
  end
end

task :run do
  if not build_type? 'debug'
    puts "[ERROR] Please run 'deploy'"
    return
  end

  if not is_built?
    Rake::Task["build"].execute
  end

  sh "cd #{build_path}; #{JAVA} -cp .:../runtime/kotlin-runtime.jar:../lib/*.jar #{PACKAGE}"
end

task :build do
  @build_type = 'debug'

  if is_built? and build_type? 'debug'
    clean_build_src
  else
    sh "mkdir -p #{build_path}"
    # sh "cp #{LIB}/* #{build_path}"
    # sh "cp #{RUNTIME_DIR}/* #{build_path}"
  end

  sh "#{KOTLINC} #{SRCFILE} -d #{build_path}"
end

task :deploy do
  @build_type = 'release'

  if is_built? and build_type? 'release'
    clean_build_src
  else
    sh "mkdir -p #{build_path} #{build_path}/lib #{build_path}/runtime #{DEPLOY}"
    sh "cp #{LIB}/* #{build_path}/lib"
    sh "cp #{RUNTIME_DIR}/* #{build_path}/runtime"
  end

  sh "#{KOTLINC} #{SRCFILE} -d #{build_path}"

  sh "mkdir -p #{DEPLOY_FILES}/META-INF"
  manifest = "#{DEPLOY_FILES}/META-INF/MANIFEST.MF"
  create_manifest(manifest)
  create_jar(manifest)
end

task :clean do
  clean_builds
  sh "rm -rf #{SRC}.jar #{SRC}.class"
end

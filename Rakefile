KOTLINC = "kotlinc-jvm"

SRC = "MyNekoze"
SRC_EXT = "kt"
SRCFILE = "#{SRC}.#{SRC_EXT}"
PACKAGE = "nekoze.NekozePackage"

LIB = "lib"
RUNTIME_DIR = "runtime"
RUNTIME = "#{RUNTIME_DIR}/kotlin-runtime.jar"
BUILD = "build"

JAVA = "java"

def is_built?
  Dir.exist? BUILD
end

def clean_builds
  if is_built?
    sh "rm -rf #{BUILD}"
  end
end

def clean_build_src
  if is_built?
    sh "rm -rf #{BUILD}/#{SRC}.class"
  end
end

task :run do
  if not is_built?
    Rake::Task["build"].execute
  end

  sh "cd #{BUILD}; #{JAVA} -cp .:../runtime/kotlin-runtime.jar:../lib/*.jar #{PACKAGE}"
end

task :build do
  if is_built?
    clean_build_src
  else
    sh "mkdir -p #{BUILD}"
    # sh "cp #{LIB}/* #{BUILD}"
    # sh "cp #{RUNTIME_DIR}/* #{BUILD}"
  end

  sh "#{KOTLINC} #{SRCFILE} -d #{BUILD}"
end

task :deploy do
  sh "#{KOTLINC} #{SRCFILE} -include-runtime -d #{SRC}.jar"
end

task :clean do
  clean_builds
  sh "rm -rf #{SRC}.jar #{SRC}.class"
end

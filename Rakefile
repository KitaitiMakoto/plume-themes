RSASS = File.join(Dir.home, ".cargo/bin/rsass")

desc "Setup build environment"
task :setup => RSASS

file RSASS do
  sh "cargo install --features=commandline rsass"
end

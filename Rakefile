require "rake/clean"

RSASS = File.join(Dir.home, ".cargo/bin/rsass")

def def_theme_task(src_file)
  theme = src_file.pathmap("%-1d-%n")
  theme_file = File.join(DEST, theme, "theme.css")
  theme_dir = File.dirname(theme_file)

  desc "Build #{theme} theme"
  task theme => theme_file

  file theme_file  => [src_file, theme_dir, :setup] do |t|
    sh "#{RSASS} --style=compressed #{t.source} > #{t.name}"
  end

  directory theme_dir

  theme
end

desc "Setup build environment"
task :setup => RSASS

file RSASS do
  sh "cargo install --features=commandline rsass"
end


THEME_FILES = FileList["src/*/*.s{a,c}ss"].exclude("src/*/_*")
DEST = "dist"
directory DEST
CLEAN.include DEST

theme_tasks = THEME_FILES.collect {|src_file| def_theme_task(src_file)}
desc "Build all themes"
task :all => theme_tasks

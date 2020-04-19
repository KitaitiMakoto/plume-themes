require "rake/clean"

RSASS = File.join(Dir.home, ".cargo/bin/rsass")

def def_theme_tasks(src_dir)
  src_files = FileList["#{src_dir}/*.s{a,c}ss"].exclude("#{src_dir}/_*")

  src_files.collect {|src_file|
    def_theme_file_task(src_file)
  }
end

def def_theme_file_task(src_file)
  src_dir = src_file.pathmap("%d")
  theme = src_file.pathmap("%-1d-%n")
  theme_dir = File.join(DEST, theme)
  theme_file = File.join(theme_dir, "theme.css")

  src_assets = FileList["#{src_dir}/**/*"].exclude("**/*/*.s{a,c}ss").select {|asset|
    File.file?(asset)
  }
  theme_assets = src_assets.collect {|src_asset|
    theme_asset = src_asset.pathmap("%{^#{src_dir},#{DEST}/#{theme}}p")
    asset_dir = theme_asset.pathmap("%d")

    file theme_asset => [src_asset, asset_dir] do |t|
      cp t.source, t.name
    end

    directory asset_dir

    theme_asset
  }

  desc "Build #{theme} theme"
  task theme => [theme_file] + theme_assets

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


DEST = "dist"
directory DEST
CLEAN.include DEST

THEMES = FileList["src/*"]

theme_tasks = THEMES.collect {|theme|
  def_theme_tasks(theme)
}.flatten
desc "Build all themes"
task :all => theme_tasks

RELEASE = `lsb_release -c | awk '{print $2}'`.strip
COMPONENT = ENV['COMPONENT'] || 'main'

S3_BUCKET = "apt.sigil66.com"
GPG_KEY_ID = '47ACDD35'
GPG_KEY_USER = 'ops@sigil66.com'

ENV['PATH'] =  "#{Dir.pwd}/.gems/bin:#{ENV['PATH']}"
ENV['GEM_HOME'] = "#{Dir.pwd}/.gems"
ENV['GEM_PATH'] = '/var/lib/gems/2.3.0'

desc 'Install Dependencies'
task :install_deps do
  Dir.mkdir "#{Dir.pwd}/.gems" unless File.directory? "#{Dir.pwd}/.gems"
  sh %{ bundle update }
end

desc 'Clean'
task :clean do
  sh %{ rm -rf ./repo }
  sh %{ rm -rf ./debian }
end

desc 'Build Apt Repo'
task :build_apt do
  sh %{ mkdir ./debian }
  sh %{ cp ../*/pkg/*.deb ./debian/ }
  if ! Dir["./debian/*"].empty?
    sh %{ bundle exec prm -t deb -p ./repo -d ./debian -c #{COMPONENT} -r #{RELEASE} -a amd64 -n SHA512 --gpg #{GPG_KEY_USER}}
  end
  # Export public key
  sh %{ gpg --output ./repo/Sigil66-Ops.gpg --armor --export #{GPG_KEY_ID} }
end

desc 'Publish Repository'
task :publish do
  sh %{ s3cmd sync ./repo/Sigil66-Ops.gpg s3://#{S3_BUCKET} }
  sh %{ s3cmd sync --delete-removed ./repo/dists/#{RELEASE} s3://#{S3_BUCKET}/dists/ }
end

task :default => [:install_deps, :clean, :build_apt, :publish]


source "https://rubygems.org"

gem "chef", path: "."

gem "ohai", git: "https://github.com/chef/ohai.git", branch: "main"

gem "chef-utils", path: File.expand_path("chef-utils", __dir__) if File.exist?(File.expand_path("chef-utils", __dir__))
gem "chef-config", path: File.expand_path("chef-config", __dir__) if File.exist?(File.expand_path("chef-config", __dir__))
gem "chef-powershell"

if File.exist?(File.expand_path("chef-bin", __dir__))
  # bundling in a git checkout
  gem "chef-bin", path: File.expand_path("chef-bin", __dir__)
else
  # bundling in omnibus
  gem "chef-bin" # rubocop:disable Bundler/DuplicatedGem
end

gem "cheffish", ">= 17"

group(:omnibus_package) do
  gem "appbundler"
  gem "rb-readline"
  gem "inspec-core-bin", "~> 4.24" # need to provide the binaries for inspec
  gem "chef-vault"
end

group(:omnibus_package, :pry) do
  # Locked because pry-byebug is broken with 13+.
  # some work is ongoing? https://github.com/deivid-rodriguez/pry-byebug/issues/343
  gem "pry", "= 0.13.0"
  # byebug does not install on freebsd on ruby 3.0
  gem "pry-byebug" unless RUBY_PLATFORM =~ /freebsd/i
  gem "pry-stack_explorer"
end

# Everything except AIX and Windows
group(:ruby_shadow) do
  # if ruby-shadow does a release that supports ruby-3.0 this can be removed
  gem "ruby-shadow", git: "https://github.com/chef/ruby-shadow", branch: "lcg/ruby-3.0", platforms: :ruby
end

# deps that cannot be put in the knife gem because they require a compiler and fail on windows nodes
group(:knife_windows_deps) do
  gem "ed25519", "~> 1.2" # ed25519 ssh key support
end

group(:development, :test) do
  gem "rake"
  gem "rspec"
  gem "webmock"
  gem "fauxhai-ng" # for chef-utils gem
end

group(:chefstyle) do
  # for testing new chefstyle rules
  gem "chefstyle", git: "https://github.com/chef/chefstyle.git", branch: "main"
end

instance_eval(ENV["GEMFILE_MOD"]) if ENV["GEMFILE_MOD"]

# If you want to load debugging tools into the bundle exec sandbox,
# add these additional dependencies into Gemfile.local
eval_gemfile("./Gemfile.local") if File.exist?("./Gemfile.local")


# PowerShell assemblies have been moved to the chef-powershell-shim repo
# require "chef-powershell"
# include Chef_PowerShell::ChefPowerShell::PowerShellExec

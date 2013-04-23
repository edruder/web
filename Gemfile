require 'configoro'

rails_root = ENV['RAILS_ROOT'] || File.dirname(__FILE__)
::Configoro.paths << File.join(rails_root, 'config', 'environments')

def conditionally(configuration_path, value, &block)
  groups = []
  dev    = Configoro.load_environment('development')
  test   = Configoro.load_environment('test')
  prod   = Configoro.load_environment('production')

  groups << :development if eval("dev.#{configuration_path}") == value
  groups << :test if eval("test.#{configuration_path}") == value
  groups << :production if eval("prod.#{configuration_path}") == value

  groups.each { |g| group g, &block }
end

source 'https://rubygems.org'

# FRAMEWORK
gem 'rails', github: 'rails/rails', branch: '3-2-stable'
# We need to use this branch of Rails because it includes fixes for ActiveRecord
# and concurrency that we need for our thread-spawning background job paradigm
# to work
gem 'configoro'
gem 'rack-cors', require: 'rack/cors'

# MODELS
gem 'pg', platform: :mri
# Version 1.2.6 introduces a bug relating to SQL binds
gem 'activerecord-jdbc-adapter', '1.2.5', platform: :jruby
gem 'activerecord-jdbcpostgresql-adapter', platform: :jruby
gem 'has_metadata_column', github: 'RISCfuture/has_metadata_column'
gem 'slugalicious'
gem 'email_validation'
gem 'url_validation'
gem 'json_serialize'
gem 'validates_timeliness'
gem 'find_or_create_on_scopes', '>= 1.2.1'
gem 'composite_primary_keys', github: 'RISCfuture/composite_primary_keys'
gem 'activerecord-postgresql-cursors'

# VIEWS
gem 'erector'
gem 'jquery-rails'
gem 'kramdown'

# UTILITIES
gem 'json'
gem 'git', github: 'RISCfuture/ruby-git'
gem 'user-agent'

# AUTH
conditionally('authentication.strategy', 'ldap') do
  gem 'net-ldap', github: 'RoryO/ruby-net-ldap', require: 'net/ldap'
end

# INTEGRATION
conditionally('jira.disabled?', false) do
  gem 'jira-ruby', require: 'jira'
end

# DOGFOOD
gem 'squash_ruby', require: 'squash/ruby'
gem 'squash_rails', require: 'squash/rails'
gem 'squash_ios_symbolicator', require: 'squash/symbolicator'
gem 'squash_javascript', require: 'squash/javascript'
gem 'squash_java', require: 'squash/java'

# BACKGROUND JOBS
conditionally('concurrency.background_runner', 'Resque') do
  gem 'resque'
  gem 'resque-pool'
end

group :assets do
  gem 'sass-rails'
  gem 'libv8', '~> 3.11.8', platform: :mri
  gem 'therubyracer', '>= 0.11.1', platform: :mri
  # Version 2.0 of TheRubyRhino breaks asset compilation
  gem 'therubyrhino', platform: :jruby
  gem 'less-rails'

  gem 'coffee-rails'
  gem 'uglifier'

  gem 'font-awesome-rails'
end

group :development do
  # DOCS
  gem 'yard', require: nil
  gem 'redcarpet', require: nil, platform: :mri
  gem 'fdoc'
end


group :test do
  # SPECS
  gem 'rspec-rails'
  gem 'factory_girl_rails'
  gem 'fakeweb'
end

gem 'sql_origin', groups: [:development, :test]

Configoro.paths.clear # reset configoro

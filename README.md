# el_finder_s3
Ruby gem to provide server side connector to elFinder using AWS S3 like a container

## Description

This is based on cdmicacc's Ruby library to provide server side ftp functionality over elFinder ruby gem of Phallstrom.

- cdmicacc project: https://github.com/cdmicacc/el_finder_ftp
- phallstrom project: https://github.com/phallstrom/el_finder


## Rails 5

Add gem 'el_finder_s3', github: 'aryalsan/el_finder_s3' to Gemfile
% bundle install

```ruby
  include ElFinderS3::Action
  
  def elfinder
    h, r = ElFinderS3::Connector.new(
      :mime_handler => ElFinderS3::MimeType,
      :root => '/',
      :url => 'CDN_BASE_PATH'
      :thumbs => true,
      :thumbs_size => 100,
      :thumbs_directory => 'thumbs',
      :home => t('admin.media.home'),
      :original_filename_method => lambda { |file| "#{File.basename(file.original_filename, File.extname(file.original_filename)).parameterize}#{File.extname(file.original_filename)}" },
      :default_perms => {:read => true, :write => true, :rm => true, :hidden => false},
      :server => {
        :bucket_name => 'BUCKET_NAME',
        :region => 'AWS_REGION',
        :access_key_id => 'ACCESS_KEY_ID',
        :secret_access_key => 'SECRET_ACCESS_KEY',
        :cdn_base_path => 'CDN_BASE_PATH'
      }
    ).run(params)
    headers.merge!(h)
    
    if r.empty?
      (render :nothing => true) and return
    end
    render :json => r, :layout => false
  end
```

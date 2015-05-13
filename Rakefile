require 'rake'

desc 'Preview the site with Jekyll'
task :preview do
    sh "jekyll serve --watch --drafts --baseurl '' --config _config.yml"
end

desc 'Search site and print specific deprecation warnings'
task :check do
    sh "jekyll doctor"
end

desc 'create a new draft post'
task :post do
  title = ENV['title']
  require 'date'
  slug = "#{Date.today}-#{title.to_s.downcase.gsub(/[^\w]+/, '-')}"

  file = File.join(
    File.dirname(__FILE__),
    '_posts',
    slug + '.markdown'
  )

  File.open(file, "w") do |f|
    f << <<-EOS.gsub(/^    /, '')
    ---
    layout: post
    title: #{title}
    categories:
    permalink:
    description:
    ---

    EOS
  end

  system ("#{ENV['EDITOR']} #{file}")
end

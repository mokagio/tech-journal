desc "Create a new empty post with the given title"
task :new, [:title] do |task, args|
  title = args[:title]
  if ! title 
    title = "an-awesome-title" 
  end

  now = Time.new
  month = now.month
  day = now.day

  if day < 10
    day = "0#{day}"
  end

  if month < 10
   month = "0#{month}"
  end

  date_string = "#{now.year}-#{month}-#{day}"
  date_time_string = "#{date_string} #{now.hour}:#{now.min}:42"

  template = <<TEMPLATE
---
layout: post
title:
date: #{date_time_string}
---
TEMPLATE

  path = "_posts/#{date_string}-#{title}.md"

  File.open(path, 'w') {|f| f.write template }
end

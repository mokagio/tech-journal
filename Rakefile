# Rake's default way to pass arguments to a task, `task[argument]`, is cumbersome
# With some hacking it's possible to have a cleaner `task argument` syntax
# More here: https://itshouldbeuseful.wordpress.com/2011/11/07/passing-parameters-to-a-rake-tas    k/
#
# We'll use this to do `calabash:test tag` and run only the tagged specs
def set_no_op_for_arguments(arguments)
  # skip the first element, which is the task itself
  for argument in arguments[1..-1] do
    # create a task that does nothing
    task argument.to_sym do ; end
  end
end

desc "Create a new empty post with the given title"
task :new do |task|
  set_no_op_for_arguments(ARGV)
  title = ARGV[1]
  raise "Missing title" unless title

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

  sh "vim #{path}"
end

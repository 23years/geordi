require 'bundler'
Bundler::GemHelper.install_tasks

desc 'Default: Run all tests'
task :default => :features

task :features do
  if Gem::Version.new(RUBY_VERSION) > Gem::Version.new('1.8.7')
    system 'bundle exec cucumber'
  else
    system 'bundle exec cucumber --tags "~@ruby>=1.9"'
  end
end

task :readme do
  require File.expand_path('../lib/geordi/cli', __FILE__)

  readme = File.read('README.md')
  geordi_section_regex = /
    `geordi`\n
    -{3,}   # 3 dashes or more
    .*?     # anything, non-greedy
    (?=     # stop before:
      ^\w+\n-{3,} # the next section
    )
  /xm

  geordi_section = <<-TEXT
`geordi`
--------

The `geordi` binary holds most of the utility commands. For the few other
binaries, see the bottom of this file.

You may abbreviate commands by typing only their first letters, e.g. `geordi
con` will boot a development console, `geordi set -t` will setup a project and
run tests afterwards.

For details on commands, e.g. supported options, you may always run
`geordi help <command>`.

  TEXT

  Geordi::CLI.all_commands.sort.each do |_, command|
    unless command.hidden?
      geordi_section << "### `geordi #{ command.usage }`\n\n"
      geordi_section << "#{ command.description.sub /(\.)?$/, '.' }\n\n"
      geordi_section << "#{ command.long_description.strip }\n\n" if command.long_description
      geordi_section << "\n"
    end
  end

  updated_readme = readme.sub(geordi_section_regex, geordi_section)
  File.open('README.md', 'w') { |f| f.puts updated_readme.strip }
  puts 'README.me updated.'
end

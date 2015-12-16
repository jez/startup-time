require 'benchmark'
require 'rake/clean'
require 'tty'

CC = 'gcc'
CLEAN.include %w[ *.out *.class .crystal ]

def which (command)
  TTY::Which.which command
end

def silent command
  system "#{command} > /dev/null 2>&1"
end

def time (test, *args)
  name = args.first

  unless ((args.size == 1) ? File.exists?(name) : which(name))
    STDERR.puts "#{name} not found, skipping"
    return
  end

  out = nil
  command = args.join ' '
  print '.'
  real = Benchmark.realtime { out = `#{command}`.chomp }

  abort "#{test}: invalid output: #{out}" unless out == 'Hello, world!'

  @times.push [ test, (real * 1000).round(1) ]
end

task :compile => %w[ hello.c.out hello.cr.out hello.go.out HelloJava.class HelloScala.class ]

if which(CC)
  file 'hello.c.out' => 'hello.c' do |t|
    sh "#{CC} -O3 -o #{t.name} #{t.source}"
  end
end

if which('crystal')
  file 'hello.cr.out' => 'hello.cr' do |t|
    sh "crystal build --release -o #{t.name} #{t.source}"
  end
end

if which('go')
  file 'hello.go.out' => 'hello.go' do |t|
    sh "go build -o #{t.name} #{t.source}"
  end
end

if which('javac')
  file 'HelloJava.class' => 'HelloJava.java' do |t|
    sh "javac #{t.source}"
  end
end

if which('scalac')
  file 'HelloScala.class' => 'HelloScala.scala' do |t|
    sh "scalac #{t.source}"
  end
end

task :default => :compile do
  @times = []

  if silent('ng ng-version') && silent('ng ng-cp .')
    time 'Java (ng)', 'ng', 'HelloJava'
  end

  time 'Bash',        'bash', 'hello.bash'
  time 'C',           'hello.c.out'
  time 'Crystal',     'hello.cr.out'
  time 'Go',          'hello.go.out'
  time 'Java',        'java', 'HelloJava'
  time 'Kotlin',      'kotlin', 'hello.kt'
  time 'Lua',         'lua', 'hello.lua'
  time 'LuaJIT',      'luajit', 'hello.lua'
  time 'Node.js',     'node', 'hello.js'
  time 'Perl',        'perl', 'hello.pl'
  time 'Python 2',    'python2', 'hello.py'
  time 'Python 2 -S', 'python2', '-S', 'hello.py'
  time 'Python 3',    'python3', 'hello.py'
  time 'Python 3 -S', 'python3', '-S', 'hello.py'
  time 'Ruby',        'ruby', 'hello.rb'
  time 'Scala',       'scala', 'HelloScala'

  sorted = @times.sort_by { |_, time| time }
  table = TTY::Table.new [ 'Program', 'Time (ms)' ], sorted
  puts $/, table.render(:basic, alignments: [ :left, :right ])
end

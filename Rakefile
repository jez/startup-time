require 'benchmark'
require 'rake/clean'
require 'tty'

CC = ENV['CC'] || 'gcc'
CLEAN.include %w[ *.out *.class .crystal META-INF ]

def which (command)
  TTY::Which.which command.to_s
end

def silent (command)
  system "#{command} > /dev/null 2>&1"
end

def time (test, *args)
  return unless args.size == 1 ? File.exists?(args.first) : which(args.first)

  out = nil
  command = args.join ' '
  print '.'
  real = Benchmark.realtime { out = `#{command}`.chomp }

  abort "#{test}: invalid output: #{out}" unless out == 'Hello, world!'

  @times.push [ test, (real * 1000).round(2) ]
end

def file_if (hash, &block)
  hash.each do |compiler, source|
    if which(compiler)
      if source.is_a?(Array)
        source, target = source
      else
        target = source =~ /^[A-Z]/ ? "#{source.pathmap('%n')}.class" : "#{source}.out"
      end

      file(target => source, &block)
      task :compile => target
    end
  end
end

file_if CC => 'hello.c' do |t|
  sh "#{CC} -O3 -o #{t.name} #{t.source}"
end

file_if crystal: 'hello.cr' do |t|
  sh "crystal build --release -o #{t.name} #{t.source}"
end

file_if dmd: 'hello.d' do |t|
  sh "dmd -O3 -o #{t.name} #{t.source}"
end

file_if gdc: 'hello.d' do |t|
  sh "gdc -O3 -o #{t.name} #{t.source}"
end

file_if go: 'hello.go' do |t|
  sh "go build -o #{t.name} #{t.source}"
end

file_if javac: 'HelloJava.java' do |t|
  sh "javac #{t.source}"
end

file_if kotlinc: [ 'HelloKotlin.kt', 'HelloKotlinKt.class' ] do |t|
  sh "kotlinc #{t.source}"
end

file_if ldc: 'hello.d' do |t|
  sh "ldc -O3 -o #{t.name} #{t.source}"
end

file_if scalac: 'HelloScala.scala' do |t|
  sh "scalac #{t.source}"
end

task :default => :compile do
  @times = []

  time 'Bash',        'bash', 'hello.bash'
  time 'C',           'hello.c.out'
  time 'Crystal',     'hello.cr.out'
  time 'D',           'hello.d.out'
  time 'Go',          'hello.go.out'
  time 'Java',        'java', 'HelloJava'
  time 'Java (ng)',   'ng', 'HelloJava' if silent('ng ng-cp .')
  time 'Kotlin',      'kotlin', 'HelloKotlinKt'
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
  table = TTY::Table.new [ 'Test', 'Time (ms)' ], sorted
  puts $/, table.render(:basic, alignments: [ :left, :right ])
end

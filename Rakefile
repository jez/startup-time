require 'benchmark'
require 'rake/clean'
require 'tty'

CC = 'gcc'

CLEAN.include(%w[ *.out *.class .crystal ])

def time (name, *args)
 if args.size > 1 && !TTY::Which.which(args.first)
  STDERR.puts "#{args.first} not found, skipping"
  return
 end

 out = nil
 real = Benchmark.realtime { out = %x[ #{args.join(' ')} ].chomp }
 abort "#{name}: invalid output: #{out}" unless out == 'Hello, world!'
 @times.push [ name, (real * 1000).round(1) ]
end

task :compile => %w[ hello.c.out hello.cr.out hello.go.out HelloJava.class HelloScala.class ]

file 'hello.c.out' => 'hello.c' do |t|
 sh "#{CC} -O3 -o #{t.name} #{t.source}"
end

file 'hello.cr.out' => 'hello.cr' do |t|
 sh "crystal build --release -o #{t.name} #{t.source}"
end

file 'hello.go.out' => 'hello.go' do |t|
 sh "go build -o #{t.name} #{t.source}"
end

file 'HelloJava.class' => 'HelloJava.java' do |t|
 sh "javac #{t.source}"
end

file 'HelloScala.class' => 'HelloScala.scala' do |t|
 sh "scalac #{t.source}"
end

task :default => :compile do
 @times = []

 time 'Bash',        'bash', 'hello.bash'
 time 'C',           'hello.c.out'
 time 'Crystal',     'hello.cr.out'
 time 'Go',          'hello.go.out'
 time 'Java',        'java', 'HelloJava'
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
 puts table.render(:basic, alignments: [ :left, :right ])
end

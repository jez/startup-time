require 'benchmark'
require 'English'
require 'rake/clean'
require 'tty'

# use the old (local) directory for Crystal build files
# so they can be purged by `rake clean`
ENV['CRYSTAL_CACHE_DIR'] = '.crystal'
CLEAN.include %w(*.out *.class .crystal META-INF)

CC              = ENV['CC'] || 'gcc'
DEVNULL         = File.open(File::NULL, 'w')
EXPECTED_OUTPUT = "Hello, world!#{$RS}"
ROUNDS          = (ENV['rounds'] || 10).to_i

def which(command)
  TTY::Which.which command.to_s
end

def time(test, *args)
  if args.size == 1
    abs_path = File.absolute_path(args.first)
    return unless File.exist? abs_path
  else
    return unless abs_path = which(args.first)
  end

  argv0 = args.shift
  cmdname = abs_path
  command = [cmdname, *args].join(' ')

  # make sure the command produces the expected output
  output = `#{command}`

  unless output == EXPECTED_OUTPUT
    abort "#{test}: invalid output: #{output.inspect}"
  end

  times = []
  print '.'

  ROUNDS.times do
    times << Benchmark.realtime { system([cmdname, argv0], *args, out: DEVNULL) }
  end

  @times << [test, times.min * 1000]
end

def file_if(hash, &block)
  hash.each do |compiler, source|
    next unless which(compiler)

    if source.is_a?(Array)
      source, target = source
    elsif source =~ /^[A-Z]/
      target = "#{source.pathmap('%n')}.class"
    else
      target = "#{source}.out"
    end

    file(target => source, &block)
    task compile: target
  end
end

file_if CC => 'hello.c' do |t|
  sh "#{CC} -O3 -o #{t.name} #{t.source}"
end

file_if crystal: 'hello.cr' do |t|
  sh "crystal build --release -o #{t.name} #{t.source}"
end

file_if dmd: 'hello.dmd.d' do |t|
  sh "dmd -O3 -o #{t.name} #{t.source}"
end

file_if gdc: 'hello.gdc.d' do |t|
  sh "gdc -O3 -o #{t.name} #{t.source}"
end

file_if go: 'hello.go' do |t|
  sh "go build -o #{t.name} #{t.source}"
end

file_if javac: 'HelloJava.java' do |t|
  sh "javac #{t.source}"
end

# XXX use kotlinc >= 1.0.0 to avoid logspam
file_if kotlinc: ['HelloKotlin.kt', 'HelloKotlinKt.class'] do |t|
  sh "kotlinc #{t.source}"
end

file_if ldc: 'hello.ldc.d' do |t|
  sh "ldc -O3 -o #{t.name} #{t.source}"
end

file_if rustc: 'hello.rs' do |t|
  sh "rustc -O -o #{t.name} #{t.source}"
end

file_if scalac: 'HelloScala.scala' do |t|
  sh "scalac #{t.source}"
end

desc 'Run the tests'
task default: :compile do
  @times = []

  time 'Bash',        'bash', 'hello.bash'
  time 'C',           'hello.c.out'
  time 'Crystal',     'hello.cr.out'
  time 'D (DMD)',     'hello.dmd.d.out'
  time 'D (GDC)',     'hello.gdc.d.out'
  time 'D (LDC)',     'hello.ldc.d.out'
  time 'Go',          'hello.go.out'
  time 'Java',        'java', 'HelloJava'
  time 'Kotlin',      'kotlin', 'HelloKotlinKt'
  time 'Lua',         'lua', 'hello.lua'
  time 'LuaJIT',      'luajit', 'hello.lua'
  time 'Node.js',     'node', 'hello.js'
  time 'Perl',        'perl', 'hello.pl'
  time 'Perl 6',      'perl6', 'hello.p6'
  time 'Python 2',    'python2', 'hello.py'
  time 'Python 2 -S', 'python2', '-S', 'hello.py'
  time 'Python 3',    'python3', 'hello.py'
  time 'Python 3 -S', 'python3', '-S', 'hello.py'
  time 'Ruby',        'ruby', 'hello.rb'
  time 'Rust',        'hello.rs.out'
  time 'Scala',       'scala', 'HelloScala'

  sorted = @times
    .sort_by { |_, time| time }
    .map { |test, time| [test, format('%.02f', time.to_s)] }

  table = TTY::Table.new ['Test', 'Time (ms)'], sorted
  puts $RS, table.render(:basic, alignments: [:left, :right])
end

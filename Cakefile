fs            = require 'fs'
CoffeeScript  = require 'coffee-script'
{spawn, exec} = require 'child_process'

# ANSI Terminal Colors.
enableColors = no
unless process.platform is 'win32'
  enableColors = not process.env.NODE_DISABLE_COLORS

bold = red = green = reset = ''
if enableColors
  bold  = '\x1B[0;1m'
  red   = '\x1B[0;31m'
  green = '\x1B[0;32m'
  reset = '\x1B[0m'


# Run a CoffeeScript through our node/coffee interpreter.
run = (args, cb) ->
  proc =         spawn 'node', ['./node_modules/.bin/coffee'].concat(args)
  proc.stderr.on 'data', (buffer) -> console.log buffer.toString()
  proc.on        'exit', (status) ->
    process.exit(1) if status != 0
    cb() if typeof cb is 'function'

# Log a message with a color.
log = (message, color, explanation) ->
  console.log color + message + reset + ' ' + (explanation or '')
  

task 'build', 'build module from source', build = (cb) ->
  files = fs.readdirSync 'src'
  files = ('src/' + file for file in files when file.match(/\.coffee$/))
  run ['-c', '-o', 'lib/'].concat(files), cb
  log ' -> build done', green
  
task 'test', 'test builded module', ->
  build ->
    test_file = 'test/run.coffee'
    run test_file, -> log ' -> all tests passed :)', green
  

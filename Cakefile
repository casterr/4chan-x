{log}  = console
{exec} = require 'child_process'
fs     = require 'fs'

VERSION = '8.0.2'

HEADER  = """
// ==UserScript==
// @name           4chan xs
// @version        #{VERSION}
// @namespace      aeosynth
// @description    Adds various features.
// @copyright      2009-2011 James Campos <james.r.campos@gmail.com>
// @copyright      2012 Nicolas Stepien <stepien.nicolas@gmail.com>
// @license        MIT; http://en.wikipedia.org/wiki/Mit_license
// @include        http://boards.4chan.org/*
// @include        https://boards.4chan.org/*
// @include        http://images.4chan.org/*
// @include        https://images.4chan.org/*
// @include        http://sys.4chan.org/*
// @include        https://sys.4chan.org/*
// @run-at         document-start
// @updateURL      https://github.com/spaghetti2514/4chan-x/raw/stable/4chan_x.user.js
// @downloadURL    https://github.com/spaghetti2514/4chan-x/raw/stable/4chan_x.user.js
// @icon           http://spaghetti2514.github.com/4chan-x/favicon.gif
// ==/UserScript==

/* LICENSE
 *
 * It's the MIT license. I assume you're all more than competent of looking it up.
 *
 * CONTRIBUTORS
 *
 * A list of contributors is available inside Mayhem's version of 4chan X.
 * Nonetheless, I appreciate everything everyone has done for this script in
 * all its incarnations.
 */


"""

CAKEFILE  = 'Cakefile'
INFILE    = 'script.coffee'
OUTFILE   = '4chan_x.user.js'
CHANGELOG = 'changelog'
LATEST    = 'latest.js'

option '-v', '--version [version]', 'Upgrade version.'

task 'upgrade', (options) ->
  {version} = options
  unless version
    console.warn 'Version argument not specified. Exiting.'
    return
  regexp = RegExp VERSION, 'g'
  for file in [CAKEFILE, INFILE, OUTFILE, LATEST]
    data = fs.readFileSync file, 'utf8'
    fs.writeFileSync file, data.replace regexp, version
  data = fs.readFileSync CHANGELOG, 'utf8'
  fs.writeFileSync CHANGELOG, data.replace 'master', "master\n\n#{version}"
  exec "git commit -am 'Release #{version}.' && git tag -a #{version} -m '#{version}' && git tag -af stable -m '#{version}'"

task 'build', ->
  exec 'coffee --print script.coffee', (err, stdout, stderr) ->
    throw err if err
    fs.writeFile OUTFILE, HEADER + stdout, (err) ->
      throw err if err

task 'dev', ->
  invoke 'build'
  fs.watchFile INFILE, interval: 250, (curr, prev) ->
    if curr.mtime > prev.mtime
      invoke 'build'

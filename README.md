[![build status](https://secure.travis-ci.org/STRML/JSXHint.png)](http://travis-ci.org/STRML/JSXHint)

#JSXHint
A wrapper around JSHint to allow linting of files containing JSX syntax.

Accepts the same input and JSHint and emits the same output. Switches sent to jsxhint
are forwarded on to jshint.

Glob parsing, ignores, and jshintrc parsing are all performed by jshint.

Automatically lints all files with the `.js` and `.jsx` extensions.

##Rationale

This module is intended for use as part of
[SublimeLinter-jsxhint](https://github.com/SublimeLinter/SublimeLinter-jsxhint),
but it can be used separately as a smart replacement for JSHint. It will automatically convert any file with
the `jsx` extension into JS using `react-tools`, then present the file to JSHint for validation.

Note that as of React 0.12, [it is recommended](https://github.com/facebook/react/issues/832) to use the `.jsx`
file extension rather than the pragma in `.js` files. If you are already using the `.jsx` extension, add the option
`--jsx-only`. This will skip attempted conversion of `.js` files to JSX, but still lint those files.
This is useful for running an entire project.

`JSXHint` is safe to use as a drop-in replacement for `JSHint`, even when JSX files are not present in a project.

`JSXHint` offers a simple workflow that accepts JSX files without modification.
Additionally, `JSXHint` actually lints the object definitions generated by the JSX compiler, allowing you to catch
mistakes in your templates (such as undefined variables, syntax errors, and missing modules).

`JSXHint` cli allows for pattern arrays via [glob-all](https://github.com/jpillora/node-glob-all)

##Examples

```
# Lint entire project. JSXHint will only lint .js and .jsx files.
jsxhint .

# Lint files in `src` folder only.
jsxhint src

# Lint Project using only `.jsx` extension for JSX.
# Will lint .js files with jshint, .jsx files with jsxhint.
jsxhint --jsx-only .

# Basic globbing
jsxhint --config ./other-directory/.jshintrc src/foo/*.jsx

# Multiple patterns
jsxhint 'jsx/**/*' '!scripts/**/*' 'scripts/outlier.jsx'

# Common multiple patterns usecase - lint .js and .jsx, ignore modules and build
jsxhint '**/*.js*' '!node_modules/**/*' '!build/**/*'

# Accepts stdin with '-'
jsxhint - < src/file.jsx

# Exclude files
jsxhint --exclude excludeme.jsx src/foo/*.jsx

# Lint project using babel (previously 6to5)
# Note that you must explicitly install `babel` if you wish to use it.
jsxhint --babel src
```

##Installation
`npm install -g jsxhint`

##Usage

```
Usage:
  jsxhint[OPTIONS] [ARGS]

Options:
  -c, --config STRING    Custom configuration file
      --reporter STRING  Custom reporter (<PATH>|jslint|checkstyle|unix)
      --exclude STRING   Exclude files matching the given filename pattern
                         (same as .jshintignore)
      --exclude-path STRINGPass in a custom jshintignore file path
      --filename STRING  Pass in a filename when using STDIN to emulate config
                         lookup for that file name
      --verbose          Show message codes
      --show-non-errors  Show additional data generated by jshint
  -e, --extra-ext STRING Comma-separated list of file extensions to use
                         (default is .js)
      --extract [STRING] Extract inline scripts contained in HTML
                         (auto|always|never, default to never)  (Default is never)
      --jslint-reporter  Use a jslint compatible reporter (DEPRECATED, use
                         --reporter=jslint instead)
      --checkstyle-reporter Use a CheckStyle compatible XML reporter
                            (DEPRECATED, use --reporter=checkstyle
                            instead)
  -v, --version          Display the current version
  -h, --help             Display help and usage details

The above options are native to JSHint, which JSXHint extends.

JSXHint Options:
      --jsx-only         Only transform files with the .jsx extension.
                         Will run somewhat faster.
      --babel            Use babel (6to5) instead of react esprima.
                         Useful if you are using es6-module, etc. You must
                         install the module `babel` manually with npm.
      --babel-experimental  Use babel with experimental support for ES7.
                            Useful if you are using es7-async, etc.
      --harmony          Use react esprima with ES6 transformation support.
                         Useful if you are using both es6-class and react.
      --es6module             Pass the --es6module flag to react tools.
      --non-strict-es6module  Pass this flag to react tools.
```

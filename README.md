# wp-textdomain
Lint and fix textdomain and usage in PHP files in plugins and themes.

## Install

Install with yarn:

```
$ yarn add wp-textdomain --dev
```

OR

Install with npm:

```
$ npm install wp-textdomain --save-dev
```

## Examples

Typical usage is done during a build process for plugins or themes.  This code shows how to add linting on your php files, and output the results to console:

```js
const wpTextdomain = require( 'wp-textdomain' );

wpTextdomain( '**/*.php', {
	domain: 'theme-name',
} );
```

 Alternatively, you can pass the configuration option to automatically fix the issues found during the linting report:

```js
const wpTextdomain = require( 'wp-textdomain' );

wpTextdomain( '**/*.php', {
	domain: 'theme-name',
	fix: true
} );
```

In some cases, you might need more than one textdomain while linting files to be checked against.  A common use case for this is by including a framework, such as Kirki, in a theme which does supply it's own translations:

```js
const wpTextdomain = require( 'wp-textdomain' );

wpTextdomain( '**/*.php', {
	domain: [ 'theme-name', 'kirki' ],
	fix: true
} );
```

When using the fix option, only errors found that aren't `kirki` are fixed.  When fixing errors, the first textdomain listed is the one used as the primary textdomain.

## Usage

```js
const wpTextdomain = require( 'wp-textdomain' );
wpTextdomain( pattern, options );
```

`pattern`: **String** A [glob pattern](https://www.npmjs.com/package/glob) for matching files.
`options`: **Object** wp-textdomain configuration options.

### Options
#### domain
**String/Array** Textdomain(s) to lint and fix.

If a string is passed, this will be the textdomain that is used to compare against in linting. When passing an array of textdomains, all are checked against as valid textdomains to be used in a theme or plugin.

When running wp-textdomain with the fix option set, the first textdomain in the array is used as the primary textdomain that is used to replace or be added to gettext calls missing or with alternate textdomains.

#### fix
**Bool** Whether or not to write files with fixes applied.

When using the fix option, errors that are found are fixed using the first textdomain listed.

#### engine
**Object** [php-parser](https://www.npmjs.com/package/php-parser) configuration options.

#### missingDomain
**Bool** Whether or not to perform lints/fixes against gettext methods that are missing textdomains.

#### variableDomain
**Bool** Whether or not to perform lints/fixes against gettext methods that are using a variable instead of a string in the textdomain field.

#### keywords
**Array** A pattern list of strings containing available gettext methods, and argument positions.

*Example*:

```js
options.keywords = [
	'__:1,2d',
	'_e:1,2d',
	'_x:1,2c,3d',
	'esc_html__:1,2d',
	'esc_html_e:1,2d',
	'esc_html_x:1,2c,3d',
	'esc_attr__:1,2d',
	'esc_attr_e:1,2d',
	'esc_attr_x:1,2c,3d',
	'_ex:1,2c,3d',
	'_n:1,2,4d',
	'_nx:1,2,4c,5d',
	'_n_noop:1,2,3d',
	'_nx_noop:1,2,3c,4d',
];
```

#### fileOpts
**Object** Configuration options for calls made to [fs.readFileSync](https://nodejs.org/api/fs.html#fs_fs_readfilesync_path_options).

#### glob
**Object** [minimatch/glob](https://www.npmjs.com/package/glob#options) configuration options to pass in.

#### force
**Bool** Option to force build fails by exiting process or just warn in console on error.

#### logfile
**Object** Logfile configuration options.
- `create` **Bool**
- `path` **String**
- `format` **Object** Filename output format.
	- `prefix` **String** String to prepend at beginning of filename.
	- `timestamp` **String** A moment.js date/time string.
	- `suffix` **String** String to append to end of filename before extension.

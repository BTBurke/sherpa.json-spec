# sherpa.json specification


### name

**Required**  
Type: `String`

The name of the package as stored in the registry.

* Must be unique.
* Should be slug style for simplicity, consistency and compatibility. Example: `unicorn-cake`
* Lowercase, a-z, can contain dash or dot but not start/end with them.
* Consecutive dashes or dots not allowed.
* 50 characters or less.


### description

*Recommended*  
Type: `String`

Any character. Max 140.

Help users identify and search for your package with a brief description. Describe what your package does, rather than what it's made of. Will be displayed in search/lookup results on the CLI and the website that can be used to search for packages.


### version

Type: `String`

The package's semantic version number.

* Must be a [semantic version](http://semver.org) parseable by [gosemver](https://github.com/myfreeweb/gosemver).
* If publishing a folder, the version must be higher than the version stored in the registry, when republishing.
* Required if publishing to the repository or use git tags.


### main

*Recommended*  
Type: `String` or `Array` of `String`

The primary acting files and directories necessary to use your package. These are used to build the source directories to execute your provisioning strategy.

* Filenames should not be versioned (Bad: script.1.1.0.sh; Good: script.sh).


### license

*Recommended*  
Type: `String` or `Array` of `String`

[SPDX license identifier](https://spdx.org/licenses/) or path/url to a license.


### ignore

*Recommended*  
Type: `Array` of `String`

A list of files for Sherpa to ignore when installing your package.

Note: README (all variants of case, .md, .text) and sherpa.json will never be ignored.

The ignore rules follow the same rules specified in the [gitignore pattern spec](http://git-scm.com/docs/gitignore).


### keywords

*Recommended*  
Type: `Array` of `String`

Same format requirements as [name](#name).

Used for search by keyword. Helps make your package easier to discover without people needing to know its name. Recommended.


### authors

Type: `Array` of (`String` or `Object`)

A list of people that authored the contents of the package.

Either:

```json
"authors": [
  "John Doe", "John Doe <john@doe.com>",
  "John Doe <john@doe.com> (http://johndoe.com)"
]
```

or:

```json
"authors": [
  { "name": "John Doe" },
  { "name": "John Doe", "email": "john@doe.com" },
  { "name": "John Doe", "email": "john@doe.com"," homepage": "http://johndoe.com" }
]
```


### homepage

Type: `String`

URL to learn more about the package. Falls back to GitHub project if not specified and it’s a GitHub endpoint.


### repository

Type: `Object`

The repository in which the source code can be found.

```json
"repository": {
  "type": "git",
  "url": "git://github.com/foo/bar.git"
}
```


### dependencies

Type: `Object`

Dependencies are specified with a simple hash of package name to a semver compatible identifier or URL.

* Key must be a valid [name](#name).
* Value must be a valid [version](#version), a Git URL, or a URL (inc. tarball and zipball).
* Value can be an owner/package shorthand, i.e. owner/package. By default, the shorthand resolves to GitHub -> https://github.com/owner/package. This will be mirrored to https://sherpa.io/owner/package.

### devDependencies

Type: `Object`

Same rules as `dependencies`.

Dependencies that are only needed for development of the package, e.g., test framework or building documentation.


### resolutions (not supported as of 0.1.0, to be added in future releases)

Type: `Object`

Dependency versions to automatically resolve with if conflicts occur between packages.

### private

Type: `Boolean`

If you set it to `true` it will publish to a private repository that is only accesible to those with permissions set on sherpa.io. This is a way to prevent accidental publication of private repositories.

### private_data  (not supported as of 0.1.0, to be added in future releases)

Type: `Object`

Private data is used to specify a local path or private repository that contains variables that are needed to fulfill or override specific data in the provisioning script.  These can be used to set environment variables, or for filling template values.  

* This can be useful in filling private values for an otherwise public script, such as for private API keys.
* Private data is only available to those with the appropriate privileges on sherpa.io.  Otherwise, public users of the recipe will enter an interactive session that will prompt for values.  These values are stored in the local version of sherpa.json.

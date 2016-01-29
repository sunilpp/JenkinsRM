# JenkinsGlobalLibs

## Summary

This LIbrary aims to develop and share a bunch of commenly used groovy utility scripts to augument jenkins workflow .
It allows to keep the utility functions available from the global lib git repo embedded in the cloudbees server.

**Warning!** The project is a **Proof of concept**. The functionality may change before the release (if it happens, of course). Use it on your own risk.

Supported features:

## Usage


### Available methods

The `fileLoader` variable provides the following methods:
* `fromGit(String libPath, String repository, String branch, String credentialsId, String labelExpression)` - loading of a single Groovy file from the specified Git repository
* `withGit(String repository, String branch, String credentialsId, String labelExpression)` - wrapper closure for multiple files loading from a same Git repo
* `load(String libPath)` - loading of an object from a Groovy file specified by the relative path. Also can be used within `withGit()` closure to load multiple objects at once

Parameters:
* `libPath` - a relative path to the file, ".groovy" extension will be added automatically
* `repository` - string representation of a path to Git repository. Supports all formats supported by [Git Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Git+Plugin)
* `branch` - Optional: Branch to be used (it's also possible to specify labels). Default value: `master`
* `credentialsId` - Optional: Credentials to be used for the Git repo checkout. Default value: `null` (unauthorized access)
* `labelExpression` - Optional: label expression, which specifies a node to be used for checkout. Default value: empty string (runs on any node)

### Groovy file format

### Examples

Loading a single Groovy file from Git:
```groovy
stage 'Load a file from GitHub'
def helloworld = fileLoader.fromGit('examples/fileLoader/helloworld', 
        'https://github.com/jenkinsci/workflow-remote-loader-plugin.git', 'master', null, '')

stage 'Run method from the loaded file'
helloworld.printHello()
```

Loading multiple files from Git:
```groovy
stage 'Load files from GitHub'
def environment, helloworld
fileLoader.withGit('https://github.com/jenkinsci/workflow-remote-loader-plugin.git', 'master', null, '') {
    helloworld = fileLoader.load('examples/fileLoader/helloworld');
    environment = fileLoader.load('examples/fileLoader/environment');
}

stage 'Run methods from the loaded content'
helloworld.printHello()
environment.dumpEnvVars()
```

## License
[MIT License](http://opensource.org/licenses/MIT)


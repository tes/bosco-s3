# Bosco-s3

Bosco is a utility knife to help manage the complexity that using microservices, which naturally results in a large number of code repositories, brings with it.  Inspired by the Github 'setup', e.g. can a developer run one simple command and get up and running?

Docs: https://bosco.readme.io/v0.4.0/docs/what-is-bosco

Please refer to the bosco documentation here https://github.com/tes/bosco, you can use this package to push assets to s3 using continuous integration without having to depend on the entire bosco.

## Command List

Commands in Bosco are defined via specific command files within the 'commands' folder: [https://github.com/tes/bosco/tree/master/commands](commands).

To get help on any command just type;

```
bosco help s3push
```

## Parameters

You can use a number of parameters to control the behaviour of Bosco.  Parameters are configuration options that can be used across commands.

|parameter|description|default|
|---------|-----------|--------|
|-e, --environment|Environment name|local|
|-b, --build|Build number or tag|default|
|-c, --configFile|Config file|~/.bosco/bosco.json|
|-p, --configPath|Config path|~/.bosco/bosco.json|
|-n, --noprompt|Do not prompt for confirmation|false|
|-f, --force|Force over ride of any files|false|
|-s, --service|Inside single service|false|
|--nocache|Ignore local cache for github projects|false|
|--offline|Ignore expired cache of remote service data and use local if available|false|

To see all possible commands and parameters, just type 'bosco'.


### S3 Push

This will create bundles for front end assets (JS, CSS, Templates), this command can be run *across* repositories in a workspace, but it is typically run within a single service (hence the -s parameter below) by a build server that dynamically assigns a build number.

```
bosco s3push -s -e <environment> -b <buildnumber>
```

This command requires that you have configured your AWS details for S3.  Best to put these into a .bosco folder in the project workspace a per environment config, e.g. .bosco/tes.json.

```json
{
    "aws": {
        "key": "XXXXXX",
        "secret": "XXXXXX",
        "bucket": "bucket-name",
        "region": "eu-west-1",
        "cdn": "https://dudu89lpwit3y.cloudfront.net"
    }
}

```

To then access the html fragments for [compoxure](https://github.com/tes/compoxure), it follows a simple convention:

```
<cdn>/<environment>/<build>/<type>/<bundle>.<fragmentType>.<js|css|html|map|txt>
```

For example:

- [https://dudu89lpwit3y.cloudfront.net/tes/55/html/bottom.js.html](https://dudu89lpwit3y.cloudfront.net/tes/55/html/bottom.js.html)

This would contain a fragment that has script a tag for all of the minified JS tagged in the bottom group.
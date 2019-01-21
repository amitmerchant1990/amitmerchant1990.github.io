---
layout: post
title: Using private repositories as a composer package in PHP
categories: [PHP, Git]
---

[Composer](https://getcomposer.org) is the goto method of adding dependencies in modern PHP application. You search for a library for a concerned functionality and you'll probably find the one in the large database of the [Pacakgist](https://packagist.org). But there comes a time where you'd want to use a library which is private and should be only available to you. Well, the good news is, you can achieve this using this trick.

This can be accomplished by specifying `repositories` you can get packages from elsewhere. This includes private repositories as well. For you to use a private repo as a composer pacakge, you need to declare the required repository in the `repositories` field in the `composer.json` of your project. Here's a sample of how you can achieve this.


```json
{
    "require": {
        "php": ">=7.0",
        "myname/mylibname": "dev"
    },
    "repositories":[
        {
            "type":"package",
            "package":{
                "name":"myname/mylibname",
                "version": "dev",
                "source":{
                    "type":"git",
                    "url":"git@bitbucket.org:myname/mylibname.git",
                    "reference":"dev"
                }
            }
        }
    ]
}
```

As shown above, By default composer will look up the packagist repository. If you define a different one in the json file this local one (`"git@bitbucket.org:myname/mylibname.git"`) will be searched first before falling back to the public repo. 

Now, run `composer update` and check the `vendor` folder. The dependency should have been installed in project and ready to use.

Note: Since, we are using private repositories here, the package will get installed only if you're using the SSH connection. This means that either you must have [setup the SSH key](https://confluence.atlassian.com/bitbucket/set-up-an-ssh-key-728138079.html) on the client or if you are like me and cant setup keys because of deployment server, you will have to use basic auth.

You can achieve basic auth by adding vcs provider's credentials in `~/.composer/auth.json`

```json
{
    "http-basic": {
        "bitbucket.org": {
            "username": "bitbucket-username",
            "password": "password"
        }
    }
}
```

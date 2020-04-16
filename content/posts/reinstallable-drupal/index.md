+++
title = "Reinstallable Drupal"
date = 2020-04-14T13:00:00+01:00
author = "John Svensson"
description = "How to set up reinstallable Drupal"
+++

Reinstallable Drupal allows you to [reinstall your Drupal site with existing configuration](https://www.drupal.org/node/2897299).

The benefits of doing so:

* Developers don't need to share databases
* You don't have to worry about broken/invalid configuration in your database when changing between Git branches
* You don't need your production database locally

## How?

If you're using a profile/distribution that doesn't implement `hook_install` (i.e. minimal) you should already be able to do this:

Using Drush:

```bash
$ vendor/bin/drush site:install --existing-config
```

Alternatively you can use the standard Drupal UI for installing after dropping the database.

## If you're using Standard profile...

Or any other profile that does implement `hook_install` you'll need to implement one of the workarounds.

### Using Install Profile Generator

The [Install Profile Generator](http://drupal.org/project/install_profile_generator) module will generate a install profile from a site's active configuration.

Download the module, install it and run the command:

```bash
$ composer require 'drupal/install_profile_generator:^3.0'
$ vendor/bin/drush en install_profile_generator
$ vendor/bin/drush install-profile-generate --name=test
```

Now you may reinstall your site using the `site:install` and `--exisiting-config` parameter using Drush.

### Set the profile to minimal post-install

The caveat of using Install Profile Generator is that all your configuration belongs in a profile. We can workaround that by installing the site with Standard and then setting the profile to i.e. minimal afterwards, which would allow you to reinstall the site, but still preserve all the great configuration Standard provides initially.

The content and post-install things Standard does, such as adding administrator role to user 1, adding shortcuts will not preserved.

Here's how you would do that:

```bash
# Install the site as usual with Standard
$ vendor/bin/drush site:install

# Export the site's active configuration
$ vendor/bin/drush cex

# Update core.extension.yml to set the profile to minimal
sed -i '' "s/standard/minimal/g" sites/default/files/config_HASH/sync/core.extension.yml

# Reinstall the site with existing configuration
$ vendor/bin/drush site:install --existing-config
```

The reinstall here is required to ensure the state of the database is correct.

That's it!
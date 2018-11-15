# Contributing to a Drupal.org project with BLT

#### Prerequisites

For a background on contributing to Drupal, please see the [Contribute to development](https://www.drupal.org/contribute/development) guide on Drupal.org.

The below assumes that all commands will be run inside the VM, with a standard BLT + DrupalVM setup. If you plan to make frequent contributions to projects on Drupal.org or are a module maintainer, it is a good idea to setup a BLT project exclusively for this purpose.

## Drupal Core

Using BTL as development environment for contributing to Drupal core can be challenging and problematic. BLT includes Drupal core by requiring in its `composer.json` file the `drupal/core` project. Drupal.org development is aligned with the `drupa/drupal` project. When working on certain issues that patch and modify Drupal core, you may be unable to apply, test, and update/re-roll patches by including them in the `patches` section of your project's root `composer.json` file. See [#2755401](https://www.drupal.org/project/drupal/issues/2755401) for an example of such issues.

## Drupal Contrib

The [Examples for Developers](https://www.drupal.org/project/examples) module will be used in the below instructions, but these should be work for any project hosted on Drupal.org, including sandbox projects.

By default, BLT will install projects in your codebase respecting the paths set in the `installer-paths` declaration in your root `composer.json`. You will need tohange directories into the install location for your project, for this example `cd docroot/modules/contrib/examples`. Note: it may be a time saver to do this in a second terminal window as you will need to toggle back to your project's root directory occasionally.

### Project contributors (non-maintainers)

#### Add the project

Patches created for contribution generally reference the HEAD of the default or other main Git branch of a project, and not a specific release tag.

Require the project in your root `composer.json` file with `composer require drupal/examples dev-1.x`.

If you have not already done so, copy the _phpcs.xlm.dist_ file in the root of your codebase to a new file that you can customize with `cp phpcs.xlm.dist phpcs.xlm`. Add a line inside the `ruleset` element for your project: `<file>docroot/modules/contrib/examples</file>`.

###### A note on code quality

Generally, a good first step before starting working on an issue is to run PHP Code Sniffer against the project code. Run `blt tests:phpcs:sniff:all` and if there are errors or warnings, consider opening a new issue, or search for an existing issue, for the project on Drupal.org and submit a patch. This will help improve the overall quality of code on Drupal.org and help reduce noise when creating your future patches to the project.

#### Working on a new issue

Identify an open issues or create a new issues on Drupal.org for your project.

##### Create a new patch

At this point, refer to the [Making a Drupal patch with Git](https://www.drupal.org/node/707484) and [Advanced patch contributor guide](https://www.drupal.org/node/1054616) pages on Drupal.org. The process of creating and working with patches for project in a BLT codebase very closely follow the standard Druapal.org workflows.

##### Cleaning up

After you have created and uploaded your patch file, it is generally a good idea to clean the state of the Git repository for your project otherwise BLT may throw errors on subsequent builds.

@TODO: git instructions to clean up.

#### Working with existing patches

If you are maintaining a patch for backwards compatibility with specific project version, you can require a specific release to work with. @TODO: Does this create a git checkout?

Add an entry to the `patches` array in your project's `composer.json` file. See BLT's patches documentation for additional information.

```
"extra": {
  "patches": {
    "drupal/examples": {
      "Update batch example to reflect new API (8.6.0)": "https://www.drupal.org/files/issues/2018-04-10/2917758-6.patch"
    }
  }
},
```
Run `composer update` to apply the patch. Note any errors in the output of the Composer process.

##### Rerolling patches

[Rerolling patches](https://www.drupal.org/patch/reroll)

##### Creating an interdiff

[Creating an interdiff](https://www.drupal.org/documentation/git/interdiff)


### Project maintainers

For a background information on maintaining a project on Drupal.org, please see the [Maintaining a drupal.org project with Git](https://www.drupal.org/node/711070) guide.

#### Adding the project

Note: In order to follow your below instructions, your Drupal.org project will need to include a [`composer.json`](https://cgit.drupalcode.org/examples/tree/composer.json) file, as detailed in [Add a composer.json file](https://www.drupal.org/node/2514612).

By default, when a project is downloaded and included in the codebase, the URLs for the Git remotes are for non-mainters.

```
$ git remote -v
composer	https://git.drupal.org/project/examples (fetch)
composer	https://git.drupal.org/project/examples (push)
origin	https://git.drupal.org/project/examples (fetch)
origin	https://git.drupal.org/project/examples (push)
```

Project maintainers can setup BLT to inclue projects using the Git URL specified on the [Version control](https://www.drupal.org/project/examples/git-instructions) page by adding a new `repository` inte the root _composer.json_ file in the codebase.

```
"repositories": {
    "examples": {
        "type": "git",
        "url": "git@git.drupal.org:project/examples.git"
    }
}
```

Require the project in your root `composer.json` file with `composer require drupal/examples dev-8.x-1.x`.

```
$ git remote -v
composer	git@git.drupal.org:project/examples.git (fetch)
composer	git@git.drupal.org:project/examples.git (push)
origin	git@git.drupal.org:project/examples.git (fetch)
origin	git@git.drupal.org:project/examples.git (push)
```

If you have not already done so, copy the _phpcs.xlm.dist_ file in the root of your codebase to a new file that you can customize with `cp phpcs.xlm.dist phpcs.xlm`. Add a line inside the `ruleset` element for your project: `<file>docroot/modules/contrib/examples</file>`.


### Code quality

Run `blt tests:phpcs:sniff:all` and if there are errors or warnings related to your change, address them and update or recreate your patch.

Run any additional linters or static analysis tools and address and warning or errors.

### Testing

If the project or you additions have test coverage, run the tests with `blt tests:phpunit:run`, through PHPUnit directly, or through the Drupal's _run-tests.sh_ script. Refer to the [Testing](testing.md) guide (and [PHPStorm](phpstorm.md) guide) for additional information. If the project does not have any test coverage, consider adding some to ensure your changes. Address and failing tests and update or recreate your patch.

Run any additional test suites such as Behat and address any failure or errors.
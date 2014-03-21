Drush Subtree
=============

Overview
--------

Drush Subtree's goal is to reduce friction for Drupal developers who want to
make their work reusable. It provides a wrapper around git-subtree to simplify
management of Git repos inside a parent repo and it provides integration with
Drush's Build Manager extension. This enables you to:

  - Use Build Manager's interactive prompt to set your site up with Git subtrees
    for projects you maintain outsite the parent site repository.

  - Do development inside whatever site repo you're actively working in,
    then push commits out of your site repo up to drupal.org or (whatever
    private repo your custom module or theme lives in).

  - Pull updates into your site repo from an outside repo for your
    distro, module, theme, or custom project.

  - For teams working together on a site repository, this enables team
    members to contribute to a project inside a site repo, through your own private
    workflow. Then it enables you--the maintainer--to easily push that work out to public
    repos when it's ready to be released.

  - Incorporate Git subtrees into Drush Make builds for your site repository.
    (Provides support for checkouting out tagged versions of projects, no commit
    ID required.)


Dependencies
------------

  - [git-subtree](https://github.com/git/git/tree/master/contrib/subtree)
  - [Build Manager](https://github.com/whitehouse/buildmanager) - Note: even if you
    don't use Build Manager to manager Drush Make builds, Drush Subtree expects
    you to store info about your subtrees in a Build Manager configuration file.
  - (Recommended) Drush master / 7.x (Build Manager uses the Drush Make
    --no-recurse flag and autoloading which--as of the time of this writing--
    ave not been backported to 6.x)

Usage
-----

Store info about your site repository's subtrees in a Build Manager
configuration file. Use the interactive prompt to have Build Manager generate
this file for you, or see examples
[here](https://github.com/whitehouse/buildmanager) to create the file manually.

Start the interactive prompt like this:

    drush buildmanager-configure

If you're already familiar with git-subtree, see documentation and examples for
the `subtree` command. This interface is the same as git subtree's with a few
simplifications (it grabs parameters from your Build Manager config, so you
don't have to type those parameters in over and over or worry about possible
human error). For more details and examples, see:

    drush subtree --help

If you're not familiar with git-subtree, you may prefer the more Drush-y
interface, `drush subtree-<verb> <args>`, see:

  drush --filter=drushsubtree

Here's a quick overview of the main commands:

    # Note: All the commands below are pushing to or pulling from a remote
    # repository specified in your Build Manager config file.

    # Add a subtree to the parent repository.
    drush subtree add <project>                

    # Pull updates in from outside your site repo.
    drush subtree pull <project>               

    # Push updates out to external repo from inside your site repo.
    drush subtree push <project>               

    # "Checkout" a tagged version of a subtree project (e.g. 7.x-2.1) inside your
    # site repository. Note: This is a faux subtree command. (It only exists via
    # Drush Subtree, there is no `git subtree checkout` command.)
    drush subtree checkout <project> <tag>

    # Specify a particular commit ID to use or "checkout" for a subtree project.
    drush subtree merge <project> <id>

Tips for getting started:

  - To see what's going on with git under the hood, use Drush's verbose flag `-v`
  - To examine shell commands generated by Drush Subtree without running them,
    use Drush's `--simulate` flag.
  - Don't include changes to multiple projects (i.e. two different modules) all
    in the same commit. For a project stored in a subtree, this enables you to push
    changes out cleanly. For projects that are NOT stored in a subtree, this
    will make it easy for you to break custom projects out of your site repo and
    make them stand-alone projects later, without losing your commit history.

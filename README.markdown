`git_berth`
===========

Provides convenient setup of post-conception resting places for new ideas.

Designed to work with SSH destinations which contain a directory heirarchy like this:

    git/
    git/category
    git/category/project
    git/category_2
    git/category_2/project

Categories ("BERTHs") could include, for example, `python_modules` or `templates`.

Upon getting berth, git_berth will register the new remote in the local git repo.

A common sequence of commands is

    $ git_berth create emergence scripts flash_of_inspiration
    $ git push emergence --all

The remote repo is configured to accept pushes into its master branch (`git config receive.denyCurrentBranch ignore`).

Until a method is provided to map git remote names to ssh hostnames, it will continue to be necessary to have a line in `~/.ssh/config` which provides a single-word alias for a given host.  For example, in this case `~/.ssh/config` might contain

    Host emergence
      HostName emergence.example.com
      User creator

The BERTH would be located in `~/git/` on the server `emergence.example.com`, resulting in the creation of a bare git repository at

    ~/git/scripts/flash_of_inspiration/

`git_berth` will break out early without creating a repository if a remote by the name HOST already exists.

### Usage:

    git_berth {create, list} ...

    Subcommands:

        git_berth create HOST BERTH PROJECT
            Creates a repo at HOST in the directory
              git/BERTH/PROJECT.
            BERTH can be a multi-level path.
            Creates a corresponding remote named HOST in the local repo.

        git_berth list HOST [BERTH]
            With BERTH: lists projects in that berth on HOST.
            Without BERTH: lists berths on HOST.

    git_berth assumes that the HOST has a directory named 'git'
      which contains berths.

    HOSTs can be any string which can both
      serve as a valid host parameter to $(ssh)
      and as the host component of a URL passed to $(git remote add).

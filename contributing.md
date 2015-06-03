Protocol
========

A guide for getting things done.

Maintain the application
------------------------

* Avoid including files in source control that are specific to your
  development machine or process.
* Delete local and remote feature branches after merging.
* Perform work in a feature branch.
* Rebase frequently to incorporate upstream changes.
* Use a [pull request](http://goo.gl/Kmdee) for code reviews.

Write a feature
---------------

Create a local feature branch based off master.

    git checkout master
    git pull
    git checkout -b <FEATURE/BUG>-<FEATURENAME>-<INITIALS>-<branch-name>

Prefix the branch name with FEATURE or BUG, then FEATURE NAME if required, then INITIALS, the branch name.

Rebase frequently to incorporate upstream changes.

    git fetch origin
    git rebase origin/master

Resolve conflicts. When feature is complete and tests pass, stage the changes.

    git add -p

When you've staged the changes, commit them.

    git status
    git commit -v

Write a [good commit message](http://goo.gl/w11us). Example format:

    Present-tense summary under 50 characters

    * More information about commit (under 72 characters).
    * More information about commit (under 72 characters).

    Github Issue #

Share your branch.

    git push origin <branch-name>

Submit a [GitHub pull request](http://goo.gl/Kmdee).

Ask for a code review in Slack.

Review code
-----------

A team member other than the author reviews the pull request. They follow
[Code Review](https://github.com/revelation/revelation-project/wiki/Code-Review) guidelines to avoid
miscommunication.

They make comments and ask questions directly on lines of code in the Github
web interface or in Slack.

For changes which they can make themselves, they check out the branch.

    git checkout <branch-name>
    git diff master..HEAD

They make small changes right in the branch, test the feature in browser,
run tests, commit, and push.

When satisfied, they comment on the pull request `Ready to merge.`

Merge
-----

Rebase interactively. Squash commits like "Fix whitespace" into one or a
small number of valuable commit(s). Edit commit messages to reveal intent.

    git fetch origin
    git rebase -i origin/master

View a list of new commits. View changed files. Merge branch into master.

    git log origin/master..<branch-name>
    git diff --stat origin/master
    git checkout master
    git merge <branch-name> --ff-only
    git push

Delete your remote feature branch.

    git push origin --delete <branch-name>

Delete your local feature branch.

    git branch --delete <branch-name>

Deploy
------

First deploy feature branch to staging for QA

    TAG=branchname cap staging deploy
    cap staging deploy:restart

If necessary, run migrations and restart the dynos.

    cap staging deploy:migrate

[Introspect](http://goo.gl/tTgVF) to make sure everything's ok.

Test the feature in browser and make sure CI isn't coming back with a failure

Deploy to production.

    cap production deploy
    cap production deploy:restart

Watch logs and metrics dashboards.

Close pull request with comment Merge

Delete feature branch

---
title: Mercurial - Common Tasks Cheat Sheet
slug: mercurial_branches_remote_repositories
date_published: 2012-09-05T11:57:00.000Z
date_updated: 2018-08-20T15:48:31.000Z
tags: Cheat Sheets, Mercurial, Source Control
---

I find myself referring back to a text file full of commands that i keep on Mercurial when performing common tasks like branching and merging.…

To create a branch called myNewFeature (notice how you must commit the branch creation, which is different to git)

    > hg branch myNewFeature 
    > hg commit -m “creating branch myNewFeature”
    

Now list branches...

    > hg branches
    

Now, lets see what current branch you are currently working on...

    > hg branch
    

When you push to a remote repository, you will need to specify that a new branch is included...

    > hg push --new-branch
    

### Switching branches

To switch your working folder to the mainline/trunk...

    > hg update default
    

To set working folder to named branch...

    > hg update yourBranchName
    

### Pulling changes from a remote repository

In order to pull from a remote repository you must have the remote repository configured in the hgrc file which is in the .hg folder in the root of your local repository. If you do not have this set up you can pass the url of the remote repository as a parameter to the pull command. If you have more than one remote repository configured you can pass the name of the entry in the hgrc file to the pull command.

First, ensure you are working on the correct branch...

    > hg branch
    

To pull the changes from the default remote repository...

    > hg pull
    

To pull changes from a named remote repository…

    > hg pull myNamedRepo 
    

At this point you will either need to merge the remote changes into your active branch, or simply update your working directory.

To update the working directory…

    > hg update 
    

To merge in the changes from remote…

    > hg merge 
    

Mercurial will merge automatically where possible, the rest you will need to merge manually using your chosen merge tool (i have [diffmerge configured](http://jorudolph.wordpress.com/2010/01/28/configuring-git-and-mercurial-to-use-diffmerge/)  for that).

Once your merge is complete, you can commit it locally...

    > hg commit -m “merging recent changes from remote repo”
    

### Pushing changes to a remote repository

Pushing to a remote repository bears much the same requirements as the pull command. In order to push to a remote repository you must have the remote repository configured in the hgrc file which is in the .hg folder in the root of your local repository. If you do not have this set up you can pass the url of the remote repository as a parameter to the push command. If you have more than one remote repository configured you can pass the name of the entry in the hgrc file to the push command.

Ensure you have any changes checked in or this will fail. To list any outstanding commits…

    > hg status 
    

Then to push to your default remote repo…

    > hg push 
    

Or, to a named remote repo…

    > hg push yourRemoteRepo 
    

### Merging 2 branches

Switch to your target branch…

    > hg update myTargetBranch 
    

Then merge in your source branch…

    > hg merge mySourceBranch 
    

As mentioned earlier - Mercurial will merge automatically where possible, the rest you will need to merge manually using your chosen merge tool (i have [diffmerge configured](http://jorudolph.wordpress.com/2010/01/28/configuring-git-and-mercurial-to-use-diffmerge/)  for that).

Once the merge is complete, commit it locally…

    > hg commit -m “merging mySourceBranch into myTargetBranch”
    

### Closing a branch

I usually [branch per feature](http://martinfowler.com/bliki/FeatureBranch.html), so i will close my branches after completing work on a new feature or set of fixes.

To close a branch, switch to it first...

    > hg update myBranchToClose 
    

Closing the branch is as simple as including an extra switch when committing to it…

    > hg commit --close-branch -m "closing myBranchToClose" 
    

Check that it no longer exists by listing all your local branches...

    > hg branches 
    

### Tagging a revision

You should always tag/label your source code when you release it.

Switch to the branch you want to tag/label, then simply issue this command…

    > hg tag my_tag
    

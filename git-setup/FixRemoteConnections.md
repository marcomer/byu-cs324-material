# Fixing Your Remote Connections

Can't fetch changes from the class repository? Fetching changes from the wrong repository? Can't pull or push changes at all? Then this document is for you.

This document references the names "CLASS\_REPO\_PATH", "PRIVATE\_REPO\_NAME", and "USERNAME". These names are meant to be replaced with an actual name:
- "CLASS\_REPO\_PATH" should be replaced with the path of the class "upstream" repository.
- "PRIVATE\_REPO\_NAME" should be replaced with the name of your own "private" repository. 
- "USERNAME" should be replaced with your GitHub username.



# Diagnosing the Issue

Running the command `git remote -v` in your local repository should be your first go to for figuring out what's gone with a remote GitHub connection. `git remote -v` prints out your remote connections to the terminal.


Typical output for `git remote -v` should similar to this:

(Replace NAMES and PATHS with their actual names)
```
origin	git@github.com:USERNAME/PRIVATE_REPO_NAME.git (fetch)
origin	git@github.com:USERNAME/PRIVATE_REPO_NAME.git (push)
upstream	git@github.com:CLASS_REPO_PATH.git (fetch)
upstream	git@github.com:CLASS_REPO_PATH.git (push)
```

Note that instead of `git@github.com:` you may have a `ssh://git@github.com:` url, which is also valid. 

Using the output from `git remote -v`, and any other errors you've received in the past, see each of the sections below for common issues and links to possible fixes.


### The URLs are incorrect
If either the origin or upstream urls link to an incorrect repository, you will need to change the url to the proper repository. See [Changing a Remote Connection](#changing-a-remote-connection).


### I have an "origin" but no "upstream", or vice versa
If `git remote -v` does not list either "origin" or "upstream", you will need to add a new remote connection. See [Adding a New Remote Connection](#adding-a-new-remote-connection).


### I have a HTTPS URL
Some students in the past have had problems pulling/pushing changes when their upstream/origin url is a HTTPS url, instead of a SSH url. If this is the case, changing the url to an SSH url may be the fix. See [Changing a Remote Connection](#changing-a-remote-connection).


### Permission Denied Error When Attempting to Pull or Push
You may be getting a "permission denied" error when attempting to pull from or push changes to GitHub. Not setting up an SSH key with your GitHub account is a common cause for this error. See our instructions on how to create and add an SSH key to GitHub in the [class repo](https://github.com/kentseamons/byu-cs324-f2024/blob/master/01a-hw-private-repo-mirror/README.md#register-an-ssh-key-for-use-with-github).

If you have registered a SSH key with GitHub and are still getting a permission denied error, it may be that your remote urls are HTTPS urls, instead of SSH urls. Using an HTTPS url will likely cause GitHub to ignore that you've registered a SSH key. See the [HTTPS URL section](#https-url) directly above.



# Changing a Remote Connection

If either your origin or upstream connections are using an incorrect URL, you will need to change the URL.

You can easily do this with the `git remote set-url` command. An example below shows how to change the URL of your **upstream** connection:

(Replace NAMES and PATHS with their actual names)
```bash
git remote set-url upstream git@github.com:CLASS_REPO_PATH.git
```

Note that if you wanted to change the URL of your **origin** connection to your private repository, you would change the command by changing `upstream` to `origin` and replacing the full `git@github.com` URL to the SSH URL of your private repository.

After running the command above, run `git remote -v` again. It should now output the correct output that you saw previously in [Diagnosing the Issue](#diagnosing-the-issue).

Now that your remote connections are correctly set, you should now test those connections. Follow the instructions in [Test Connections and Update Your Mirrored Repository from the Upstream](#test-connections-and-update-your-mirrored-repository-from-the-upstream).



# Adding a New Remote Connection

If you are missing either your origin or upstream connections, you will need to add a new connection.

You can easily do this with the `git remote add` command. An example below shows how to add a new **upstream** connection:

(Replace NAMES and PATHS with their actual names)
```bash
git remote add upstream git@github.com:CLASS_REPO_PATH.git
```

Note that if you wanted to add an **origin** connection to your private repository, you would change the command by changing `upstream` to `origin` and replacing the full `git@github.com` URL to the SSH URL of your private repository.

After running the command above, run `git remote -v` again. It should now output the correct output that you saw previously in [Diagnosing the Issue](#diagnosing-the-issue).

Now that your remote connections are correctly set, you should now test those connections. Follow the instructions in [Test Connections and Update Your Mirrored Repository from the Upstream](#test-connections-and-update-your-mirrored-repository-from-the-upstream).



# Test Connections and Update Your Mirrored Repository from the Upstream

Once you are sure `git remote -v` is showing that your remote connections are correct and that you have an "origin" and "upstream" connection, you should then test your connections by doing the typical process for updating your mirrored repo from the upstream.

Run the commands below. If you run into any errors along the way, go back to [Diagnosing the Issue](#diagnosing-the-issue) and try to determine what went wrong. If you still can't figure out what's wrong, make a post on Discord and/or talk with the TAs.

 1. Pull down the latest changes from both your repository and the upstream:

    ```bash
    git fetch --all
    ```

 2. Stash (save aside) any uncommitted changes that you might have locally in
    your clone:

    ```bash
    git stash
    ```

 3. Merge in the changes from the upstream repository:

    ```bash
    git merge upstream/master
    ```
    NOTE: after running this merge command, git may tell you that you have a **merge conflict** that you need to resolve. I would like to create a document detailing how to resolve merge conflicts, but for right now you will have to follow online tutorials to see how it's done. One word of advice for resolving merge conflicts: you will likely want to accept the *incoming* changes.
    TODO: could you get other errors? rebase. needs further testing

 4. Merge back in any uncommitted changes that were stashed:

    ```bash
    git stash pop
    ```

 5. Push out the locally merged changes to your repository:

    ```bash
    git push
    ```

If you are still having issues with your remote connections, make a post on Discord and/or talk with the TAs.
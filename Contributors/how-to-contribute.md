# How to contribute

## Before you get started

### Sign the CLA

Click the **Sign in with Github to agree** button to sign the CLA.

What is [CLA](https://www.apache.org/licenses/contributor-agreements.html)?

## Step 1: Fork in the Cloud

1. Visit https://github.com/vesoft-inc/nebula
1. Click `Fork` button (top right) to establish a cloud-based fork.

## Step 2: Clone Fork to Local Storage

Define a local working directory:

```bash
# Define your working directory
working_dir=$HOME/Workspace
```

Set `user` to match your Github profile name:

```bash
user={your Github profile name}
```

Create your clone:

```bash
mkdir -p $working_dir
cd $working_dir
git clone https://github.com/$user/nebula.git
# the following is recommended
# or: git clone git@github.com:$user/nebula.git
cd $working_dir/nebula
git remote add upstream https://github.com/vesoft-inc/nebula.git
# or: git remote add upstream git@github.com:vesoft-inc/nebula.git
# Never push to upstream master since you do not have write access.
git remote set-url --push upstream no_push
# Confirm that your remotes make sense:
# It should look like:
# origin    git@github.com:$(user)/nebula.git (fetch)
# origin    git@github.com:$(user)/nebula.git (push)
# upstream  https://github.com/vesoft-inc/nebula (fetch)
# upstream  no_push (push)
git remote -v
```

### Define a Pre-Commit Hook

Please link the **Nebula Graph** pre-commit hook into your `.git` directory.

This hook checks your commits for formatting, building, doc generation, etc.

```bash
cd $working_dir/nebula/.git/hooks
ln -s ../../.linters/cpp/hooks/pre-commit.sh .
```

Sometimes, pre-commit hook can not be executable. You have to make it executable manually.

```bash
cd $working_dir/nebula/.git/hooks
chmod +x pre-commit
```

## Step 3: Branch

Get your local master up to date:

```bash
cd $working_dir/nebula
git fetch upstream
git checkout master
git rebase upstream/master
```

Checkout a new branch from master:

```bash
git checkout -b myfeature
```

**NOTE**: Because your PR often consists of several commits, which might be squashed while being merged into upstream,
we strongly suggest you open a separate topic branch to make your changes on. After merged,
this topic branch could be just abandoned, thus you could synchronize your master branch with
upstream easily with a rebase like above. Otherwise, if you commit your changes directly into master,
maybe you must use a hard reset on the master branch, like:

```bash
git fetch upstream
git checkout master
git reset --hard upstream/master
git push --force origin master
```

### Step 4: Develop

#### Code Style

We adopt `cpplint` to make sure that the project conforms to Google's coding style guides. The checker will be implemented before the code is committed.

#### Unit Tests Required

Please add unit tests for your new features or bugfixes.

#### Build Your Code with Unit Tests Enable

Please refer to the [build source code](../3.build-develop-and-administration/1.build/1.build-source-code.md) documentation to compile.

> Make sure you have enabled the build of unit tests by setting `-DENABLE_TESTING=ON`.
#### Run Tests

In the root folder of `nebula` , run the following command:

```bash
ctest -j$(nproc)
```

### Step 5: Bring Your Branch Update to Date

```bash
# While on your myfeature branch.
git fetch upstream
git rebase upstream/master
```

You need to bring the head branch up to date after other collaborators merge pull requests to the base branch.

### Step 6: Commit

Commit your changes.

```bash
git commit
```

Likely you'll go back and edit/build/test some more than `commit --amend` in a few cycles.

### Step 7: Push

When ready to review (or just to establish an offsite backup or your work),
push your branch to your fork on `github.com`:

```bash
git push origin myfeature
```

### Step 8: Create a Pull Request

1. Visit your fork at https://github.com/$user/nebula (replace `$user` obviously).
2. Click the `Compare & pull request` button next to your `myfeature` branch.

### Step 9: Get a Code Review

Once your pull request has been opened, it will be assigned to at least two
reviewers. Those reviewers will do a thorough code review to make sure that the changes meet the repository's contributing guidelines and other quality standards.
# GIT Notes

## Remove commit history in git
reference http://stackoverflow.com/questions/9683279/make-the-current-commit-the-only-initial-commit-in-a-git-repository

```sh

rm -rf .git

git init
git add .
git commit -m "Initial commit"


git remote add origin <github-uri>
git push -u --force origin master

```


git config user.name "John Wick"
git config user.email "jwick@continental.com"


## To the experimental branch
http://stackoverflow.com/questions/2944469/how-to-commit-my-current-changes-to-a-different-branch-in-git

In master

```sh
git stash
git checkout -b experimental
git stash pop

git commit -m "Put to experimental, sht didnt workout"

git push -u origin experimental

```

## Merged it back to the master

```sh
git checkout master
git merge experimental
git push -u origin master

```

When doing another experimental branch
make the original experimental branch even with the master by merging the master to it or by deleting that branch and re-creating it again from the master


### Set remote url

```
git remote set-url origin https://github.com/USERNAME/OTHERREPOSITORY.git
```


### See the changelog based on commits

```
git log --oneline --decorate --color
```


### set a Global gitignore

```
git config --global core.excludesfile ~/.gitignore_global
```


### Show history of file
```
git log --follow -p README.md
```


## How to restore detached HEAD
```
git checkout -- master
```

- Make the head a new branch
- checkout the master
- merge the new branch to master


## Adding a submodule and attach to the master, by default it is detached from the head
the -b master argument attache the HEAD to the master branch
```
git submodule add -b master https://github.com/my/repository.git
```

## To attach the HEAD to the master branch
```sh
cd rustorm
git checkout master
```

## Update submodule repo
```sh
git submodule update --remote
```

## How to configure git to avoid accidental git push to remote origin
This is useful for having a commercial version aside from the community edition version

```sh
git config remote.origin.receivepack /bin/false
```

Alternatively, you can put this on the .gitconfig file
```.gitconfig
[merge]
  ff = false
[pull]
  ff = only
```
## You can also set the url of the push to be a non-valid one.
```sh
$ git remote set-url --push origin no-pushing
$ git push
fatal: 'no-pushing' does not appear to be a git repository
fatal: The remote end hung up unexpectedly
```
## You can also set the push url to something like this.
```
git remote set-url --push origin -- --read-only--
```

## Tag a git commit version and push
```sh
git tag 0.1.0
git push origin master --tags
```

## Push a specific tag
```sh
git push origin 0.1.0
```


##  Delete a tag
```sh
git push --delete origin tagname

If you also need to delete the local tag, use:

git tag --delete tagname

```

## Tell git to store globally store the github credential, the next time you
push to a repo it will not ask again for the credentials
```
git config --global credential.helper 'store --file ~/.git-credentials'
```

and then in ~/.git-credentials
```
https://jwick:mypwd@gitlab.com
https://jwick:mypwd@github.com
```

## If it doesn't work use the cache helper
git config --global credential.helper 'cache'

## Checkout a a github PR to your local workstation before doing anything
```
git fetch origin pull/ID/head
```
so, to checkout PR#42
```
git fetch origin pull/42/head
```

to checkout PR#42 into a new branch pr-42
```
git fetch origin pull/42/head:pr-42
git checkout pr-42
```


## Blame authors for this file who introduce writing "unsafe" code
```sh
git blame mod.rs | grep "unsafe"
```

## Replace your local repo with the origin/master branch
```
git reset --hard origin/master
```

## Git conventions

Semver version: MAJOR.MINOR.PATCH

Given a version number MAJOR.MINOR.PATCH, increment the:
 1. MAJOR version when you make incompatible API changes,
 2. MINOR version when you add functionality in a backwards compatible manner, and
 3. PATCH version when you make backwards compatible bug fixes.

### Format

```
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

### Allowed <type>:
build:
ci:
chore: maintenance
docs: documentations
feat: a commit of the type feat introduces a new feature to the codebase, correlates with the MINOR in semver
fix: a commit of the type fix patches a bug in your codebase, correlates with PATCH in semver
perf: performance improvements
refactor: When renaming, moving modules around
revert:
style: formatting, missing semi colons, ...
test: when adding missing tests

### Allowed <scope>

Scope could be anything specifying place of the commit change. For example stargate, signer, hash, common,


## <subject> text
- use imperative, present tense: “change” not “changed” nor “changes”
- don't capitalize first letter
- no dot (.) at the end

## 99% for the git commands examples
https://bitbucket.org/BitPusher16/dotfiles/raw/49a01d929dcaebcca68bbb1859b4ac1aea93b073/refs/git/git_examples.sh

## How to write git commit messages
https://github.com/joelparkerhenderson/git_commit_message


## Add gpg verified commits in github

- To generate a gpg key
    ```
    $> gpg --full-generate-key
    ```
- To list down gpg kesy
    ```
    $> gpg --list-secret-keys --keyid-format LONG

    sec   rsa3082/16B6568258A225E6 2020-03-09 [SC] [expires: 2022-03-09]
          EEE9C3570C3CA78CD00D560B16B6568258A114E6
    uid                 [ultimate] Jovansonlee Cesar <jwick@continental.com>
    ssb   rsa3072/C345253BABCC600F 2020-03-09 [E] [expires: 2022-03-09]
    ```
- To export the public key
    ```
    $> gpg --armor --export EEE9C3570C3CA78CD00D560B16B6568258A114E6

     -----BEGIN PGP PUBLIC KEY BLOCK-----

     mQGNBF5mdw0BDAC+0v+PPOaDoTiPsnmIlyYo5jqD6NNhT8YNss52UV3i5urJ+VVe
     kcfJ9vgFIA6OCznNwZEjVMCGJltVbD83NNcyIs8sx6K9LHxBEFKvRm+SGdIA0N1/
     ...
     -----END PGP PUBLIC KEY BLOCK-----
    ```
- To enable signed commit in github, store the gpg public key in github settings
- Change the git url format to
    ```
    git remote set-url origin git@github.com:username/repo.git
    ```
- To delete a key

   ```sh
    $> gpg --delete-secret-key 16B6568258A114E6
    $> gpg --delete-key 16B6568258A114E6

   ```

- To assign globally which signingkey to be used in your repo
    ```sh
    $> git config --global user.signingkey EEE9C3570C3CA78CD00D560B16B6568258A114E6

    ```
## Error: `git error: gpg failed to sign the data`, fatal: failed to write commit object
- Make sure your system is using gpg2.
- The old key might have had expired, thus you need to generate a new one.

    ```sh
    sudo apt install gnupg2
    ```

## Adding large files in github repository
```
git lfs install
git lfs track "*.psd"
git add .gitattributes
git add file.psd
git commit -m "Add design file"
git push origin main
```


## git compare file changes
```sh
git config diff.tool vimdiff
git difftool
```


## Count the number of lines in git project
```sh
git ls-files | xargs wc -l
```


## Error: git@github.com: Permission denied (publickey).  fatal: Could not read from remote repository.

```ssh
ssh-keygen
```
copy the contents of `cat ~/.ssh/id_rsa.pub` into github ssh settings


## Git conventional commit link
https://www.conventionalcommits.org/en/v1.0.0/

## show the changes that has just been staged
```
git diff --cached
```


## Amend the last commit message
```
git commit --amend -m "fix: this will change the last commit message"
```

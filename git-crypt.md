# How to use git-crypt

```
# initialise the repo with git-crypt (generate encryption key)
git-crypt init

# add collaborators
git-crypt add-gpg-user <GPG_ID>

# specify file encryption rules (wildcards)
# example of .gitattributes:
vault_* filter=git-crypt diff=git-crypt

# add and commit
```

# git-config-switcher

How do  I manage having many gpp signatures / git identies ? 


### cheatsheet

```bash 
# deactivate signature, globally, and locally, before switching ops
# 
git config --global commit.gpgsign false
git config commit.gpgsign false


# list available gpg signatures on the machine

gpg --list-public

# switch from one signature, to another

git config --global --list

# work at cresh 
git config --global commit.gpgsign true
git config --global user.name "Jean-Baptiste Lasselle"
git config --global user.email jean-baptiste@cresh.eu
git config --global user.signingkey B058780A3258C5D4

git config --global --list

# work on my personal projects
git config --global commit.gpgsign true
git config --global user.name "Jean-Baptiste-Lasselle"
git config --global user.email jean.baptiste.lasselle.pegasus@gmail.com
git config --global user.signingkey 7B19A8E1574C2883

git config --global --list

```


* My git config for work at cresh.eu (through gitlab.com, verified email address, related to https://keybase.io/jblassellecresh ) : 

```bash
user.name=Jean-Baptiste Lasselle
user.signingkey=B058780A3258C5D4
user.email=jean-baptiste@cresh.eu
commit.gpgsign=true
```

* My git config for work on my personal repos (through github and gitlab, github and gitlab same verified email address, and same GPG Key, related to https://keybase.io/jblasselle , all very important for identity proofs ) : 

```bash
user.name=Jean-Baptiste-Lasselle
user.signingkey=7B19A8E1574C2883
user.email=jean.baptiste.lasselle.pegasus@gmail.com
commit.gpgsign=true
```


See how my personal (not work) github and gitlab identities appear in Gtilab.com and Github.com account details : 

![github bourne identity](ccc)

![gitlab bourne identity](ccc)



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
export GIT_SSH_COMMAND='ssh -i ~/.ssh.perso.backed/id_rsa'
ssh -Ti ~/.ssh.perso.backed/id_rsa git@github.com
ssh -Ti ~/.ssh.perso.backed/id_rsa git@gitlab.com
```

* git ssh tests output example : 

```bash 
jbl@poste-devops-jbl-16gbram:~/k3s-topgun$ ssh -Ti ~/.ssh.perso.backed/id_rsa git@github.com
Hi Jean-Baptiste-Lasselle! You've successfully authenticated, but GitHub does not provide shell access.
jbl@poste-devops-jbl-16gbram:~/k3s-topgun$ 
```

* my personal SSH Keypairs are saved into a private (no members added) gitlab.com repository (so I do not ever loose them) (improvement : switch to a proper secret manager, like hashicorp vault, which has drp capabilities 0.000000001 % unrecoverable loss probability), especially `~/.ssh.perso.backed/id_rsa`



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


* See how my personal (not work) github and gitlab identities appear in Gitlab.com and Github.com account details : 

![github bourne identity](https://github.com/Jean-Baptiste-Lasselle/git-config-switcher/raw/master/docs/images/GITHUB_GPG_KEY_VERIFIED_EMAILS_Firefox_Screenshot_2020-07-04T17-10-26.250Z.png)

![gitlab bourne identity](https://github.com/Jean-Baptiste-Lasselle/git-config-switcher/raw/master/docs/images/GITLAB_GPG_KEY_VERIFIED_EMAILS_2020-07-04T17-17-13.320Z.png)

* And the related keybase.io accounts : 

  * for my personal works : 
![keybase.io personal bourne identity](https://github.com/Jean-Baptiste-Lasselle/git-config-switcher/raw/master/docs/images/KEYBASE_IO_PERSONAL_BOURNE_ID_2020-07-04%2019-25-25.png)

  * for my work in my employers company, cresh.eu :
![keybase.io cresh bourne identity](https://github.com/Jean-Baptiste-Lasselle/git-config-switcher/raw/master/docs/images/KEYBASE_IO_CRESH_BOURNE_ID_2020-07-04%2019-24-51.png)


### How to create a GPG Key linked to a keybase.io account

TODO (but very smple, you'll find that on the web)

### How to verify a signature against a GPG Public Key

* for a git commit : TODO
* for any signed file : TODO

### How to sign any file with your GPG Key (and people can check your signature using your GPG Public Key )

TODO (also very simple, just search the web, or go to [this issue I opened in helm project](https://github.com/helm/helm/issues/7838) )




### How to re-geenerate my SSH keys for 2 identities, and for both gitlab.com and github.com

```bash

# ---- > FOR MY BOURNE IDENTITY AT WORK AT CREHS.EU => GITLAB.COM ONLY
# regenerate a key pair for my identity at work at cresh.eu
ssh-keygen -t rsa -b 2048 -C  "jbl@cresh.eu" -N "" -P "" -f ~/.ssh/id_rsa
# important : need to add this to the ssh-agent, or gitlab will complain
ssh-add ~/.ssh/id_rsa

# ---- > FOR MY personal BOURNE IDENTITY IN MY PERSONAL PEROJECTS => GITLAB.COM AND GITHUB.COM
ssh-keygen -t rsa -b 2048 -C  "jbl@cresh.eu" -N "" -P "" -f ~/.ssh.perso.backed/id_rsa
# important : need to add this to the ssh-agent, or gitlab will complain
ssh-add ~/.ssh.perso.backed/id_rsa

```

* personal github and gitlab :
  * https://github.com/Jean-Baptiste-Lasselle
  * https://gitlab.com/pegasus.devops

* professional github and gitlab :
  * https://gitlab.com/jean-baptiste4
  * https://github.com/jblasselle-creshdevops
  

# git config switcher

How do  I manage having many Identities across git service providers ? 

Using : 

* SSH Keys,
* GPG Keys
* keybase.io service, and its proof of identity feature
* github.com and gitlab.com, and their : 
  * email verification features
  * SSH Keys features
  * GPGP Keys features

> Done on a Debain Linux workstation


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

# -----
# work at cresh 
git config --global commit.gpgsign true
git config --global user.name "Jean-Baptiste Lasselle"
git config --global user.email jean-baptiste@cresh.eu
git config --global user.signingkey B058780A3258C5D4

git config --global --list

export GIT_SSH_COMMAND='ssh -i ~/.ssh.perso.backed/id_rsa'
ssh -Ti ~/.ssh/id_rsa git@github.com
ssh -Ti ~/.ssh/id_rsa git@gitlab.com

# -----
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
jbl@poste-devops-jbl-16gbram:~/k3s-topgun$ ssh -Ti ~/.ssh.perso.backed/id_rsa git@gitlab.com
Welcome to GitLab, @pegasus.devops!
jbl@poste-devops-jbl-16gbram:~/k3s-topgun$ ssh -Ti ~/.ssh/id_rsa git@github.com
Hi jblasselle-creshdevops! You've successfully authenticated, but GitHub does not provide shell access.
jbl@poste-devops-jbl-16gbram:~/k3s-topgun$ ssh -Ti ~/.ssh/id_rsa git@gitlab.com
Welcome to GitLab, @jean-baptiste4!
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




### How to reset my SSH keys for 2 identities, and for both gitlab.com and github.com

* regenerate and ssh-agent register both SSH keys :

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

* Then add them to my accounts  (On all 4 accounts, `Settings -> SSH Keys` menus, easy to find)

* personal github and gitlab :
  * https://github.com/Jean-Baptiste-Lasselle
  * https://gitlab.com/pegasus.devops

* professional github and gitlab :
  * https://gitlab.com/jean-baptiste4
  * https://github.com/jblasselle-creshdevops

* Last, and most important :
  * secure all the keys in a private `HashiCorp Vault` that has 1,2,3 DRP (one copy on site, one copy remote site in cloud)
  * automate secret rotation on a 15 days basis, inspired by hashcorp's execllent seth vargo example scripts based on: 
    * https://github.com/sethvargo/vault-secrets-gen (backed at https://gitlab.com/second-bureau/bellerophon/hashicorp-vault/password-rotation/vault-secrets-gen )
    * and https://github.com/scarolan/painless-password-rotation  (backed at https://gitlab.com/second-bureau/bellerophon/hashicorp-vault/password-rotation/vault-secrets-gen ) 
    * (Hey, i'm gonna change my password today !!! so excited ! ;) ) @seancarolan


#### The hashicorp vault password rotator 

Specs (design requirements) : 

* No external software. Works with Bash or Powershell.
* Trusted source of entropy for password generation.
* Each machine responsible for its own password rotation.
* Supports random secure passwords or passphrases.
* All passwords are encrypted in transit and at rest.
* Servers have write-only access to HashiCorp Vault.
* `N` number of previous password versions stored in Vault.
* Humans are granted read access to passwords by policy.
* Passwords are automatically rotated every X days.


* install vault https://learn.hashicorp.com/vault/ as a K8S deploymeent into a K3S I run at home
* install the password generator plugin fro vault : 

```bash 
git clone https://github.com/sethvargo/vault-secrets-gen
cd vault-secrets-gen/
# build from source
# TODO
# Move the compiled plugin into Vault's configured plugin_directory
# (so I will build a docker iamge FROM vault:${VAULT_DESIRED_VERSION} - MULTISTAGE BUILD HERE to 
# run the build from source, and use GOLANG as first stage of the multi stage)
# 
mv vault-secrets-gen /etc/vault/plugins/vault-secrets-gen

# Enable mlock so the plugin can safely be enabled and disabled:
# hum, set_cap has to be configured at the docker level, see how to do that for a K8S pod => ccc
setcap cap_ipc_lock=+ep /etc/vault/plugins/vault-secrets-gen

# ---
# Calculate the SHA256 of the plugin and register it in Vault's plugin catalog.
# If you are downloading the pre-compiled binary, it is highly recommended that you use the published checksums to verify integrity.
# ( see HashiCorp Vault's GPG public Key and https://github.com/hashicorp/terraform/issues/24498 )
export SHA256=$(shasum -a 256 "/etc/vault/plugins/vault-secrets-gen" | cut -d' ' -f1)

vault plugin register \
  -sha256="${SHA256}" \
  -command="vault-secrets-gen" \
  secret secrets-gen
  
# --- 
# Last step : 
# Mount the secrets engine:
# 
vault secrets enable \
  -path="gen" \
  -plugin-name="secrets-gen" \
  plugin
  
```
* now create a version 2 KV store in VAult (version 2, cause it enables versioning of secrets, very important for our secret rotation feature) : 

```bash 
vault secrets enable -version=2 -path=systemcreds/ kv
```
* Now, when we use Vault, all ops are just REST API calls. For which we will use a Token, like al API. The token used should have the following policy permissions to be able to generate passwords  : 

```
path "gen/password" {
  capabilities = ["create", "update"]
}
```

* `rotate-linux.hcl` : 
 ```
 # Allows hosts to write new passwords
path "systemcreds/data/linux/*" {
  capabilities = ["create", "update"]
}
# Allow hosts to generate new passphrases
path "gen/passphrase" {
  capabilities = ["update"]
}
# Allow hosts to generate new passwords
path "gen/password" {
  capabilities = ["update"]
}
```

* `linuxadmin.hcl` : 

```
# Allows admins to read passwords.
path "systemcreds/*" {
  capabilities = ["list"]
}
path "systemcreds/data/linux/*" {
  capabilities = ["list", "read"]
}
```
* `rotate-windows.hcl` : 

```
# Allows hosts to write new passwords
path "systemcreds/data/windows/*" {
  capabilities = ["create", "update"]
}
# Allow hosts to generate new passphrases
path "gen/passphrase" {
  capabilities = ["update"]
}
# Allow hosts to generate new passwords
path "gen/password" {
  capabilities = ["update"]
}
```
* `windowsadmin.hcl` : 

```
# Allows admins to read passwords.
path "systemcreds/*" {
  capabilities = ["list"]
}
path "systemcreds/data/windows/*" {
  capabilities = ["list", "read"]
}
```


* Okay, now we are ready to work with the password rotator : 

  * Create a new token for each machine you want rotation of passwowrds for. You'll probably want to script this part if you have a large number of machines. So install them (teh scripts) to all machines, and add them to the user's PATH env. variables (the users : thsoe users you want password rotation for)
  
 ```bash
 vault token create -policy=rotate-linux -period=24h
 ```
 
   * also configure persistent env. varaibles needed by those scripts : 

```bash
export VAULT_ADDR=https://your.vaultserver.com:8200
export VAULT_TOKEN=762XkidjlyxyzBwYqPGKWAAE
```

  * And run the scripts that rotate the passwords : 
    * linux  : 
```bash
export USER_WHO_NEED_PASSWORD_ROTATION=root
./rotate-linux-password.sh ${USER_WHO_NEED_PASSWORD_ROTATION}

```
    * windows power shell : 
```bash 
./rotate-windows-password.ps1 Administrator
```

* Renew the Vault token : 
  * linux : 

```bash 
curl -sS --fail -X POST -H "X-Vault-Token: $VAULT_TOKEN" ${VAULT_ADDR}/v1/auth/token/renew-self | grep -q 'lease_duration'
retval=$?
if [[ $retval -ne 0 ]]; then
  echo "Error renewing Vault token lease."
fi
```
  * windows power shell : 

```bash 
Invoke-RestMethod -Headers @{"X-Vault-Token" = ${VAULT_TOKEN}} -Method POST -Uri ${VAULT_ADDR}/v1/auth/token/renew-self
if(-Not $?)
{
   Write-Output "Error renewing Vault token lease."
}
```
* generate a new password : 
  * linux geenrate new pasword : 
```bash 
NEWPASS=$(curl -sS --fail -X POST -H "X-Vault-Token: $VAULT_TOKEN" -H "Content-Type: application/json" --data '{"words":"4","separator":"-"}'  ${VAULT_ADDR}/v1/gen/passphrase | jq -r '.data|.value')
JSON="{ \"options\": { \"max_versions\": 12 }, \"data\": { \"${USERNAME}\": \"$NEWPASS\" } }"
```
  * linux set the new password 

```bash 
# First commit the new password to vault, then capture the exit status
curl -sS --fail -X POST -H "X-Vault-Token: $VAULT_TOKEN" --data "$JSON" ${VAULT_ADDR}/v1/systemcreds/data/linux/$(hostname)/${USERNAME}_creds | grep -q 'request_id'
retval=$?
if [[ $retval -eq 0 ]]; then
  # After we save the password to vault, update it on the instance
  echo "$USERNAME:$NEWPASS" | sudo chpasswd
  retval=$?
    if [[ $retval -eq 0 ]]; then
      echo -e "${USERNAME}'s password was stored in Vault and updated locally."
    else
      echo "Error: ${USERNAME}'s password was stored in Vault but *not* updated locally."
    fi
else
  echo "Error saving new password to Vault. Local password will remain unchanged."
  exit 1
fi
```



  * windows power shell generate the new password:
  
```
$NEWPASS = (Invoke-RestMethod -Headers @{"X-Vault-Token" = ${VAULT_TOKEN}} -Method POST -Body "{`"length`":`"36`",`"symbols`":`"0`"}" -Uri ${VAULT_ADDR}/v1/gen/password).data.value
$SECUREPASS = ConvertTo-SecureString $NEWPASS -AsPlainText -Force
$JSON="{ `"options`": { `"max_versions`": 12 }, `"data`": { `"$USERNAME`": `"$NEWPASS`" } }"
```

  * windows power shell set the new password for the user : 

```
Invoke-RestMethod -Headers @{"X-Vault-Token" = ${VAULT_TOKEN}} -Method POST -Body $JSON -Uri ${VAULT_ADDR}/v1/systemcreds/data/windows/${HOSTNAME}/${USERNAME}_creds
if($?) {
   Write-Output "Vault updated with new password."
   $UserAccount = Get-LocalUser -name $USERNAME
   $UserAccount | Set-LocalUser -Password $SECUREPASS
   if($?) {
       Write-Output "${USERNAME}'s password was stored in Vault and updated locally."
   }
   else {
       Write-Output "Error: ${USERNAME}'s password was stored in Vault but *not* updated locally."
   }
}
else {
    Write-Output "Error saving new password to Vault. Local password will remain unchanged."
}
```

# The Final Question
 
 And how would you do that if suddenly there was no internet anymore ?
 
 To be or not to be , that, my friend, is the question.
 

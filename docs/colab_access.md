# Google Colab access to SPL repositories

__Note1__:notebook:: This document consists of how to access private Github Organisation repositories in Google Colab.

__Warning__:warning:: You have to have access to SPL private repos to clone.

__Note2__:notebook:: If any errors occurs, contact the developer team.

## Instructions:

1. In order to access any of the organisation's private repositories programmatically on any Machine or instance (Google Colab), you need to have created [fine-grained personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-fine-grained-personal-access-token). Click the link and follow the instructions. At the end, make sure to copy the `access token` and store it in a `github_access_token.txt` file, and upload that to `content/drive/MyDrive/secrets/` folder in Google drive.

2. Open a Colab note book andMount the drive.

```
from google.colab import drive
drive.mount('/content/drive')
```

3. Install GitHub CLI in Colab along with necessary software.

```
# Installing necessary software
!apt-get update -y
!apt-get upgrade -y
!apt-get install wget
!type -p curl >/dev/null || (apt update && apt install curl -y)
# Github CLI install
!curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg \
&& chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg \
&& echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
&& apt update \
&& apt install gh -y
```

4. Access GitHub with personal access token

```
# Checking GitHub CLI installation
!gh --version
# GitHub auth login with token
access_token_file = os.path.join("/content/drive/MyDrive/secrets/github_access_token.txt")
if not os.path.exists(access_token_file):
  print(f"Please click the link below and create personal access token and put that in {access_token_file}")
  print("https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-fine-grained-personal-access-token")
else:
  !gh auth login --with-token < $access_token_file
```

5. Generate SSH key par and add it to the GitHub SSH keys.

```
# Generate SSH Key Pair
!ssh-keygen -t rsa -b 4096
!ssh-keyscan -t rsa github.com >> ~/.ssh/known_hosts
# Add SSH key to SPL Repository Deploy Keys
!gh ssh-key add /root/.ssh/id_rsa.pub --type authentication
!ssh -T git@github.com
```

6. Add GitHub Credentials

```
# Git credentials
user_name = input("Enter GitHub user name:")
user_email = input("Enter GitHub user email:")
!git --version
!git config --global user.name $user_name
!git config --global user.email $user_email
```

7. Clone Space Park Leicester private repositories in Google Colab

```
# Git clone PLanetSPL repository
!git clone git@github.com:SpaceParkLeicester/PlanetSPL.git
```


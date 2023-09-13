# Jenkins Install on Linux(OS)


1. This is the Debian package repository of Jenkins to automate installation and upgrade. To use this repository, first add the key to your system:

```bat
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
    /usr/share/keyrings/jenkins-keyring.asc > /dev/null
```

Before you add jenkins key to your system
```bat
xxxxxx@xxxxxxx:/usr/share/keyrings$ ls
ubuntu-advantage-cc-eal.gpg            ubuntu-advantage-ros.gpg
ubuntu-advantage-cis.gpg               ubuntu-archive-keyring.gpg
ubuntu-advantage-esm-apps.gpg          ubuntu-archive-removed-keys.gpg
ubuntu-advantage-esm-infra-trusty.gpg  ubuntu-cloudimage-keyring.gpg
ubuntu-advantage-fips.gpg              ubuntu-cloudimage-removed-keys.gpg
ubuntu-advantage-realtime-kernel.gpg   ubuntu-master-keyring.gpg
```

After adding jenkins key
```bat
taylee@taylee-PC:/usr/share/keyrings$ ls
jenkins-keyring.asc                    ubuntu-advantage-ros.gpg
ubuntu-advantage-cc-eal.gpg            ubuntu-archive-keyring.gpg
ubuntu-advantage-cis.gpg               ubuntu-archive-removed-keys.gpg
ubuntu-advantage-esm-apps.gpg          ubuntu-cloudimage-keyring.gpg
ubuntu-advantage-esm-infra-trusty.gpg  ubuntu-cloudimage-removed-keys.gpg
ubuntu-advantage-fips.gpg              ubuntu-master-keyring.gpg
ubuntu-advantage-realtime-kernel.gpg
```

---------
2. Then add a Jenkins apt repository entry
```bat
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
    https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
    /etc/apt/sources.list.d/jenkins.list > /dev/null
```


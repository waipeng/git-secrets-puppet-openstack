# Summary

This repository holds the prohibited patterns that can be used with [git-secrets](https://github.com/awslabs/git-secrets). The patterns here are hiera keys for [puppet openstack modules](https://docs.openstack.org/puppet-openstack-guide/latest/).

# Description

git-secrets works on a list of prohibited patterns and allowed patterns (regex). This repository holds the prohibited patterns, more specifically hiera __keys__ that should not have a clear text __value__. They look like this:

```
magnum::keystone::authtoken::password
neutron::server::notifications::password
```

The list of allowed patterns will be set in each repository, in `.gitallowed`. This is the pattern of your whatever encryption method that you use to encrypt the hieradata. For example, if you are using hiera-eyaml and encrypting using gpg. the pattern will look like:

```
^.*ENC\[GPG.*$
```

After setting up a puppet site repo with the patterns, a `git secrets --scan` will pick up hieradata with keys in prohibited list, and removes those that are encrypted.

# Usage

## Local scanning

1. Follow the installation instructions on [git-secrets](https://github.com/awslabs/git-secrets) to install the git plugin.
2. Clone this repo
3. Change directory to the repo to scan
    ```
    cd <repo_to_scan>
    ```
4. Set the provider to one of the files in this repo. We set it to `all` as this holds all the prohibited patterns.
    ```
    git secrets --add-provider -- cat <path>/git-secrets-puppet-openstack/all
    ```
5. Add the allowed pattern to the current repository. This can be set to whatever encryption method used for your current repository.
6. For example, you can do the following if your hieradata is GPG encryoted
    ```
    echo '^.*ENC\[GPG.*$' > .gitallowed
7. Start the scan
    ```
    git secrets --scan
    ```

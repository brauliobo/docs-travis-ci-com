---
title: Custom Deployment
layout: en
permalink: /user/deployment/custom/
---

You can easily deploy to your own server the way you would deploy from your local machine by adding a custom [`after_success`](/user/build-configuration/#Define-custom-build-lifecycle-commands) step.

You may choose the [Script provider](/user/deployment/script/) instead, as it provides
easier flexibility with conditional deployment.

### FTP

    env:
      global:
        - "FTP_USER=user"
        - "FTP_PASSWORD=password"
    after_success:
        "curl --ftp-create-dirs -T uploadfilename -u $FTP_USER:$FTP_PASSWORD ftp://sitename.com/directory/myfile"

The env variables `FTP_USER` and `FTP_PASSWORD` can also be [encrypted](/user/encryption-keys/).

See [curl(1)](http://curl.haxx.se/docs/manpage.html) for more details on how to use cURL as an FTP client.

### Git

This should also work with services you can deploy to via git.

    after_success:
      - chmod 600 .travis/deploy_key.pem # this key should have push access
      - ssh-add .travis/deploy_key.pem
      - git remote add deploy DEPLOY_REPO_URI_GOES_HERE
      - git push deploy

See ["How can I encrypt files that include sensitive data?"](/user/travis-pro/#How-can-I-encrypt-files-that-include-sensitive-data%3F) if you don't want to commit the private key unencrypted to your repository.

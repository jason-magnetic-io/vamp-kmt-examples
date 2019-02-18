# vamp-kmt-examples

## Setup

You will need:
* A [GitHub account](https://github.com/join)
* A public SSH key to use with your GibHub account
* A [CircleCI account](https://circleci.com/signup/)
* (Optional) A [Docker Hub account](https://hub.docker.com/signup)

### Step 1: Fork the this repo
Fork this repo by pressing the **Fork** button at the top right of this page

### Step 2: Fork the `sava-cart` repo
Fork the `sava-cart` [repo](https://github.com/magneticio/sava-cart)

### Step 3: Setup the projects on CircleCI
#### `vamp-kmt-examples`
* Go to `https://circleci.com/add-projects/gh/you` on CircleCI and setup the `vamp-kmt-examples` project
* Click the **Set Up Project** button next to `vamp-kmt-examples`
   * Make sure the **Operating System** is _Linux_
   * Scroll down and click the **Start building** button
#### `sava-cart`
* Go to `https://circleci.com/add-projects/gh/you` on CircleCI and setup the `sava-cart` project
* Click the **Set Up Project** button next to `sava-cart`
   * Make sure the **Operating System** is _Linux_
   * Scroll down and click the **Start building** button
* Go to `https://circleci.com/gh/you/sava-cart/edit#env-vars` on CircleCI
* Click the **Add Variable** button
   * Enter `GITHUB_EMAIL` in the **Name** field
   * Enter the email address you use for your GitHub account in the **Value** field
   * Click **Add Variable**
* (Optional) To enable publishing of the Docker images, set the following environment variables
   * Add `PUBLISH_DOCKER_IMAGE` and set the value to `true`
   * Add `DOCKER_ORG` and set the value to name of your Docker Hub account username or organization
   * Add `DOCKER_USER` and set the value to name of your Docker Hub account username
   * Add `DOCKER_PASS` and set the value to name of your Docker Hub account password

### Step 4: Add a read/write deploy key for `vamp-kmt-examples`
When you add a new project on CircleCI, a deployment key is automatically created in GitHub for your project. The deployment key is read-only, so CircleCI cannot push to your repository with the key.

To see the benefit of `vamp-kmt` we need the build pipeline for the services to trigger an automated canary-release to all the applications that depend on that service. To do that, the service builds need to push changes to this repository. That cannot be done using a read-only deployment key.

You can manually add a read/write deployment key with the following steps:
1. Create a ssh key pair by following the [GitHub instructions](https://help.github.com/articles/connecting-to-github-with-ssh/#generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)  
   **Note:** when you are asked to "Enter passphrase (empty for no passphrase)", do _**not**_ enter a passphrase. You _**do not**_ need to add the key to your SSH agent
2. Go to `https://github.com/you/vamp-kmt-examples/settings/keys` on GitHub and click **Add deploy key**
   * Enter `CircleCI` in the **Title** field
   * Copy and paste the public key you just created into the **Key** field  
   _Linux:_ `xclip -sel clip < ~/.ssh/id_rsa.pub`  
   _Mac:_ `pbcopy < ~/.ssh/id_rsa.pub`  
   _Windows:_ `clip < ~/.ssh/id_rsa.pub`
   * Make sure **Allow write access** is set, then click **Add key**
3. Go to `https://circleci.com/gh/you/sava-cart/edit#ssh` on CircleCI and and click **Add SSH Key**  
   **Note:** you are adding the key to the `sava-cart` project because we want the `sava-cart` build to push changes to `vamp-kmt-examples`.
   * Enter `github.com` in the **Hostname** field
   * Copy and paste the private key you just created into the **Private Key** field  
   _Linux:_ `xclip -sel clip < ~/.ssh/id_rsa`  
   _Mac:_ `pbcopy < ~/.ssh/id_rsa`  
   _Windows:_ `clip < ~/.ssh/id_rsa`
   * Click **Add SSH Key**
   * Make a copy of the fingerprint, you will need it for the next step

### Step 5: Add the SSH key `sava-cart` CircleCI config
* Edit `sava-cart/.circleci/config.yml`
   * On line 28, replace `"<your-fingerprint>"` with the fingerprint you copied in the previous step  
   _For example_: replace `"<your-fingerprint>"` with `"ab:cd:ef:01:23:45:67:89:a0:b1:c2:d3:e4:f5:96:87"`
* Commit and push your change to GitHub


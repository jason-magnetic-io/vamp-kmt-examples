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
* Go to `https://circleci.com/add-projects/gh/you` on CircleCI and setup the `vamp-kmt-examples` project
* Click the **Set Up Project** button next to `vamp-kmt-examples`
   * Make sure the **Operating System** is _Linux_
   * Scroll down and click the **Start building** button
* Go back to `https://circleci.com/add-projects/gh/you` on CircleCI and setup the `sava-cart` project
* Click the **Set Up Project** button next to `sava-cart`
   * Make sure the **Operating System** is _Linux_
   * Scroll down and click the **Start building** button

### Step 4: Add a read/write deploy key for `vamp-kmt-examples`
When you add a new project on CircleCI, a deployment key is automatically created in GitHub for your project. The deployment key is read-only, so CircleCI cannot push to your repository with the key.

To see the benefit of `vamp-kmt` we need the build pipeline for the services to trigger an automated canary-release to all the applications that depend on that service. To do that, the service builds need to push changes to this repository. That cannot be done using a read-only deployment key.

You can manually add a read/write deployment key with the following steps:
1. Create a ssh key pair by following the [GitHub instructions](https://help.github.com/articles/generating-ssh-keys/)  
   **Note:** when you are asked to "Enter passphrase (empty for no passphrase)", do _**not**_ enter a passphrase.
2. Go to `https://github.com/you/vamp-kmt-examples/settings/keys` on GitHub and click **Add deploy key**
   * Enter `CircleCI` in the **Title** field
   * Copy and paste the public key you just created into the **Key** field
   * Make sure **Allow write access** is set, then click **Add key**
3. Go to `https://circleci.com/gh/you/sava-cart/edit#ssh` on CircleCI and and click **Add SSH Key**  
   **Note:** you are adding the key to the `sava-cart` project because we want the `sava-cart` build to push changes to `vamp-kmt-examples`.
   * Enter `github.com` in the **Hostname** field
   * Copy and paste the private key you just created into the **Private Key** field
   * Click **Add SSH Key**



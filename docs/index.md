# Getting Started on Digitize

This references and should not supercede the [Digitize Tutorial](https://confluence.il2.dso.mil/display/puckboardhelp/Common+API%3A+How+to+use+your+Scratch+Space+in+a+Digitize+Application) guide. I tried to write this in a way that should be a bit more accessible to beginners.

> If at any point you get stuck, you can email help@dsop.io to get help.

> A common error while getting started is finding that you do not have the permission to access a site, e.g. code.il2.dso.mil or jira.il2.dso.mil etc... You should contact help@dsop.io to get a ticket created for you.

## Step 0: IL2 Mattermost

You will need to log in to the Platform One hosted IL2 Mattermost server: <https://chat.il2.dso.mil/>.

> _Learn more about Impact Levels from GSA Cloud Information Center [here](https://cic.gsa.gov/basics/cloud-security/)._

>_Learn how to register your account with Platform One [here](https://login.dso.mil/)._

## Step 1: Request a new Digitize project

When you first join the IL2 Mattermost server you will enter the `welcome` team, and the `Town Square` channel within that.

You need to join the Tron team on Mattermost. To do so, **type** the following in the text input line:

``` mattermost
/requestaccess team tron-air-force
```

> You must **type** the command in, you cannot copy/paste it. There is a command palette that will appear when you type the first `/`.

Once your request has been approved, you will be added to the Tron team and placed into the `Town Square` channel.

Click on the `+` button in the left navigation bar (next to the team name, Tron) to open the channel list. Find the `Digitize - Public` team and join it. This is the place to ask questions and get help related to Digitize, CI/CD, and deployment of your project.

In the channel header there will be a link to `Request a new Digitize Repository`. At time of writing, the link is a google form [here](https://docs.google.com/forms/d/e/1FAIpQLSemenfdx9U6ftW3UfYKUySO4e0S1MMoqKgmI4WRw543PzIg4w/viewform).

If you do not hear a response within a working day or two, you can post in the `Digitize - Public` channel and ask for help.

## Step 2: Configure your repository

You will likely receive a DM in Mattermost with the link to your initialized repository. If your project was named `my-project`, expect the repository will be located at: `https://code.il2.dso.mil/tron/products/dod-open-source/digitize/my-project`.

Open the [Digitize Tutorial](https://confluence.il2.dso.mil/display/puckboardhelp/Common+API%3A+How+to+use+your+Scratch+Space+in+a+Digitize+Application) guide to learn how to configure your repository.

Expect that you will adjust several CI/CD settings and add several environment variables. Precisely which you need to set will depend on what type of site you are building. 

### Build Tools

The guide is designed for a React App, but you can use the following systems to build your site:

- [Vanilla JS/HTML/CSS](https://code.il2.dso.mil/tron/products/digitize/digitize-pipeline/-/blob/master/vanilla.yml)
- [npm](https://code.il2.dso.mil/tron/products/digitize/digitize-pipeline/-/blob/master/npm.yml)
- [yarn](https://code.il2.dso.mil/tron/products/digitize/digitize-pipeline/-/blob/master/yarn.yml)
- [mkdocs](https://code.il2.dso.mil/tron/products/digitize/digitize-pipeline/-/blob/master/mkdocs.yml)
- [hugo](https://code.il2.dso.mil/tron/products/digitize/digitize-pipeline/-/blob/master/hugo.yml)

> The available CI/CD pipeline config files are viewable on the Digitize repository [here](https://code.il2.dso.mil/tron/products/digitize/digitize-pipeline).

### Env Variables

The following environment variables are required for the CI/CD pipeline to run:

> Reference the [Digitize Tutorial](https://confluence.il2.dso.mil/display/puckboardhelp/Common+API%3A+How+to+use+your+Scratch+Space+in+a+Digitize+Application) for details and a more complete discussion of these environment variables.

- BUILD_OUTPUT_DIR => Required for npm, yarn, mkdocs, hugo
- DIGITIZE_DEPLOYMENT_TYPE => branch or tag
- DIGITIZE_REGISTRY_ACCESS_TOKEN => reference docs. This value is in not unique to your project and is listed in the docs.
- DIGITIZE_IL_TYPE => IL4, Digitize only supports IL4 deployment at this time.
- SKIP_E2E_TEST => Set to true to skip running e2e tests. They're broken right now.

### Protected Branches

This was the weird thing for me.

> Reference: [GitLab protected branch docs](https://docs.gitlab.com/ee/user/project/protected_branches.html).

The idea is that there are **two** types of pipelines. Full Staging/Deployment pipleines and a mini-pipeline for branches. The only difference is that the mini-pipeline does not push to staging or production.

The full pipeline can **only** be used on your `master` branch.

The mini pipelines will **only** run on protected branches.

You can protect a specific branch, but that is cumbersome. GitLab lets you define a pattern with wildcard matching that will match the branch name and then protect that branch. For example a branch called `fix/fix-rendering-issue` would be protected with the pattern `fix/*`.

Furthermore, you cannot merge a non-protected branch into `master`.

This forces a branch-naming convention because your branches must comply with these patterns otherwise they will not build or merge.

> **Recommendation**: Protect patterns `fix/*` and `feature/*` for your branches. This lets you work on new capabilities within the `feature` namespace and bug fixes within the `fix` namespace.

> **Recommendation**: Allow Maintainers to merge and Developers+Maintainers to push.

### React specific

You need to add a `homepage` field to your `package.json` file and set its value to `"."`. This is required for running the React App in a nested path.

**Congratulations**, your repository should be ready to begin building your site!

## Step 3: Configure your development environment

There are a few things you need to do to get your development environment ready to work with IL2 GitLab.

### GPG signing

**THIS IS A CHALLENGE ON GFE DEVICES**. I do not yet have a good solution.

https://code.il2.dso.mil enforces GPG signing for all commits. This means you need to create a GPG key on your workstation and add it to your GitLab account.

GPG keys in GitLab are connected with your user profile, not a specific repository. You can find your key in your [profile settings](https://code.il2.dso.mil/-/profile).

GitLab has a help page for signing commits with GPG [here](https://code.il2.dso.mil/help/user/project/repository/gpg_signed_commits/index.md). I will not repeat these steps here.

### Personal Access Token

By far the easiest way to push commits to the [code.il2.dso.mil](code.il2.dso.mil) repository is to use a personal access token.

A personal access token is like a password that lets your computer talk to the [code.il2.dso.mil](code.il2.dso.mil) server. You can create one [here](https://code.il2.dso.mil/-/profile/personal_access_tokens).

You can use the same token for all of your [code.il2.dso.mil](code.il2.dso.mil) repositories.

You will give the token a name, this is just so you can differentiate it from a separate token. The token will need `write_repository` and `read_repository` permissions at a minimum.

When you create the token, you will be presented the text string that is your personal access token.

#### Simplifying login

There are many ways to manage personal access tokens ([e.g.](https://docs.github.com/en/get-started/getting-started-with-git/caching-your-github-credentials-in-git)). A way to simplify this is to prepend the token to the `git remote` url so that your pushes are automatically authenticated. If you do not, you should get a prompt asking for the token value when you attempt to push. 

> Example: Your token is `123abc` and your repository is `code.il2.dso.mil/tron/products/dod-open-source/digitize/my-project.git` then you would set your remote to `https://oath2:123abc@code.il2.dso.mil/tron/products/dod-open-source/digitize/my-project.git`.

**CONGRATULATIONS**, your development environment should be ready to go!

### Other pieces for your workstation

If you're developing and use the react/npm toolchain, you will want to have `node.js` and `npm` installed so you can preview your app locally. There are numerous tutorials available for how to do this.

## Step 4: Get a Scratch Space assigned

Scratch space is a server-side key-value store that allows for data persistence. It is part of the Tron Common UI API. 

You need to post in the Tron/Arcade channel to get a Scratch Space assigned to you. You will be the `SCRATCH_ADMIN` for the scratch space.

## Gotchas

match your GPG key exactly to the email address that you use to sign in to code.il2.dso.mil.
user.name
user.email

git config --local user.signingkey <your key>

### Node.js version

The build will run using Node.js v16.3.0. I ran into issues because I originally built my project locally using `npx create-react-app` while using `node18` and `React 18`. This got messy.

**Better Solution**: Clone the provided repo to your local machine and start from there. That is a version that will successfully build with the CI/CD pipeline.
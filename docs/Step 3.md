# Step 3: Configure your development environment

There are a few things you need to do to get your development environment ready to work with IL2 GitLab.

## GPG signing

**THIS IS A CHALLENGE ON GFE DEVICES**. I do not yet have a good solution.

https://code.il2.dso.mil enforces GPG signing for all commits. This means you need to create a GPG key on your workstation and add it to your GitLab account.

GPG keys in GitLab are connected with your user profile, not a specific repository. You can find your key in your [profile settings](https://code.il2.dso.mil/-/profile).

GitLab has a help page for signing commits with GPG [here](https://code.il2.dso.mil/help/user/project/repository/gpg_signed_commits/index.md). I will not repeat these steps here.

## Git Config

**TODO: I just found this useful tutorial for managing a `Work` and `Personal` git config profile. The steps below do not use this method, but I think its pretty good and I might switch over to it as well!**

You will need to add several git config entries

- `user.name`
- `user.email`
- `user.signingkey`
- `commit.gpgsign`

And possibly

- `gpg.program`

### Git Config, what is it?

First, a bit of background on git config. The [official docs](https://git-scm.com/docs/git-config) are always a good place to start.

The general idea is that git config supplies a bunch of configuration variables that git references. There are two scopes, `global` and `local` with `local` overriding any conflicting configurations from `global`. The `global` scope automatically applies to all git repositories, old, existing, and new. The `local` scope only applies to the repository you're currently in.

### Ask yourself, do I feel like a developer today?

My setup is configured with my `global` values for `user.*` set to my personal GitHub account. This causes a problem because [code.il2.dso.mil](https://code.il2.dso.mil) requires GPG signing. 

> Your _GPG key_ must be associated with a _GitLab verified email_ that matches your _committer identity_.

This means all three pieces have to line up:

1. The email associated with your GPG key
2. The email associated with your GitLab login (your .mil email)
3. The email associated with `git config`

You **cannot** change the GitLab login email, which means your GPG key and `git config` _must_ be your .mil email account.

The problem for my case is that my `git config --global` has `user.email` set to my personal email account, not my .mil.

**IF** you only develop within the [code.il2.dso.mil](https://code.il2.dso.mil) repository, then you should set your global git config values to your .mil email address and there is no problem. However, if like me, you do not wish to do this, skip to

### The solution, git config --local

The solution is to use `git config --local` when setting values. This will let you provide overriding values that are scoped just to the current repository.

### Actually setting git config

The command you'll need to use is the following:

=== "Global"
      ```shell
      git config user.name 'First Last'
      ```
=== "Local"
      ```shell
      git config --local user.name 'First Last'
      ```

depending on your global/local preference.

The name and email values are self explanatory. To get the value for `user.signingkey` you need to run `gpg -k --keyid-format short`. On Linux it will look like this:

```console
foo@bar:app$ gpg -k --keyid-format short
/home/bar/.gnupg/pubring.kbx
-----------------------------
sec   rsa4096/ABCDEFGH 2022-01-01 [SC]
              ^------^ <------------------------- This is what you want
      ABCDEFGHABCDEFGHABCDEFGHABCDEFGHABCDEFGH
uid         [ultimate] John Doe (comment) <your-email@something.mil>
ssb   rsa4096/ABCDEFGH 2022-01-01 [E]
```
In this example you grab the 8 digits after `rsa4096` on the key with `[SC]`. That would be `ABCDEFGH`. This string is the short reference to your key. 

```shell
git config user.signingkey ABCDEFGH
```
The final setting that you need to configure is:
```shell
git config commit.gpgsign true
```
This will ensure that you sign every commit. If your commits are not signed, then your pushes will be rejected by [code.il2.dso.mil](https://code.il2.dso.mil).

## Personal Access Token

By far the easiest way to push commits to the [code.il2.dso.mil](https://code.il2.dso.mil) repository is to use a personal access token.

A personal access token is like a password that lets your computer talk to the [code.il2.dso.mil](https://code.il2.dso.mil) server. You can create one [here](https://code.il2.dso.mil/-/profile/personal_access_tokens).

You can use the same token for all of your [code.il2.dso.mil](https://code.il2.dso.mil) repositories.

You will give the token a name, this is just so you can differentiate it from a separate token. The token will need `write_repository` and `read_repository` permissions at a minimum.

When you create the token, you will be presented the text string that is your personal access token.

## Simplifying login

**I NEED TO CHECK THIS MAY NOT BE REQUIRED**
**THIS IS NOT GOOD PRACTICE AS YOUR PAT IS STORED IN PLAINTEXT**
=== "Linux"
      ```shell
      git config --global credential.helper libsecret
      ```
=== "Windows"
      ```shell
      git config --global credential.helper manager
      ```
=== "OS X"
      ```shell
      git config --global credential.helper osxkeychain
      ```
(!!Windows untested, could be wincred instead of manager, reference this [stack overflow thread](https://stackoverflow.com/questions/5343068/is-there-a-way-to-cache-https-credentials-for-pushing-commits/18362082#18362082)!!)

There are many ways to manage personal access tokens ([e.g.](https://docs.github.com/en/get-started/getting-started-with-git/caching-your-github-credentials-in-git)). A way to simplify this is to prepend the token to the `git remote` url so that your pushes are automatically authenticated. If you do not, you should get a prompt asking for the token value when you attempt to push. 

> Example: Your token is `123abc` and your repository is `code.il2.dso.mil/tron/products/dod-open-source/digitize/my-project.git` then you would set your remote to `https://oath2:123abc@code.il2.dso.mil/tron/products/dod-open-source/digitize/my-project.git`.

!!!success "Congratulations"
      Your development environment should be ready to go!

## Other pieces for your workstation

If you're developing and use the react/npm toolchain, you will want to have `node.js` and `npm` installed so you can preview your app locally. There are numerous tutorials available for how to do this.

## Dockerized Development

This is an alternative to setting up your local environment. Consider this when using anything other than vanilla JS/HTML/CSS.

This will take advantage of VS Code's Docker extension and ability to run within a docker container.

First you'll create a Dockerfile in your project.

Example:

```dockerfile
FROM node:16.15.0
ENV NODE_ENV=development
WORKDIR /usr/src/app
COPY ["package.json", "package-lock.json*", "npm-shrinkwrap.json*", "./"]
RUN npm install --silent && mv node_modules ../
COPY . .
EXPOSE 3000
RUN chown -R node /usr/src/app
USER node
CMD ["npm", "start"]
```

Then in VS Code you use the command palette to `Remote-Container: Open folder in container...` Then select your Dockerfile as the basis for the container. Your container image will be built and then VS Code will reopen, but this time from within the container. When you run `npm run start` it should automatically forward the port to your local machine. If not, there is a PORTS tab next to the terminal where you can manually add the necessary port forwarding.
# Step 3: Configure your development environment

There are a few things you need to do to get your development environment ready to work with IL2 GitLab.

## GPG signing

**THIS IS A CHALLENGE ON GFE DEVICES**. I do not yet have a good solution.

https://code.il2.dso.mil enforces GPG signing for all commits. This means you need to create a GPG key on your workstation and add it to your GitLab account.

GPG keys in GitLab are connected with your user profile, not a specific repository. You can find your key in your [profile settings](https://code.il2.dso.mil/-/profile).

GitLab has a help page for signing commits with GPG [here](https://code.il2.dso.mil/help/user/project/repository/gpg_signed_commits/index.md). I will not repeat these steps here.

## Personal Access Token

By far the easiest way to push commits to the [code.il2.dso.mil](code.il2.dso.mil) repository is to use a personal access token.

A personal access token is like a password that lets your computer talk to the [code.il2.dso.mil](code.il2.dso.mil) server. You can create one [here](https://code.il2.dso.mil/-/profile/personal_access_tokens).

You can use the same token for all of your [code.il2.dso.mil](code.il2.dso.mil) repositories.

You will give the token a name, this is just so you can differentiate it from a separate token. The token will need `write_repository` and `read_repository` permissions at a minimum.

When you create the token, you will be presented the text string that is your personal access token.

## Simplifying login

**I NEED TO CHECK THIS MAY NOT BE REQUIRED**

There are many ways to manage personal access tokens ([e.g.](https://docs.github.com/en/get-started/getting-started-with-git/caching-your-github-credentials-in-git)). A way to simplify this is to prepend the token to the `git remote` url so that your pushes are automatically authenticated. If you do not, you should get a prompt asking for the token value when you attempt to push. 

> Example: Your token is `123abc` and your repository is `code.il2.dso.mil/tron/products/dod-open-source/digitize/my-project.git` then you would set your remote to `https://oath2:123abc@code.il2.dso.mil/tron/products/dod-open-source/digitize/my-project.git`.

**CONGRATULATIONS**, your development environment should be ready to go!

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
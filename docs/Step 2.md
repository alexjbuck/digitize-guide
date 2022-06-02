# Step 2: Configure your repository

You will likely receive a DM in Mattermost with the link to your initialized repository. If your project was named `my-project`, expect the repository will be located at: `https://code.il2.dso.mil/tron/products/dod-open-source/digitize/my-project`.

Open the [Digitize Tutorial](https://confluence.il2.dso.mil/display/puckboardhelp/Common+API%3A+How+to+use+your+Scratch+Space+in+a+Digitize+Application) guide to learn how to configure your repository.

Expect that you will adjust several CI/CD settings and add several environment variables. Precisely which you need to set will depend on what type of site you are building. 

## Build Tools

The guide is designed for a React App, but you can use the following systems to build your site:

- [Vanilla JS/HTML/CSS](https://code.il2.dso.mil/tron/products/digitize/digitize-pipeline/-/blob/master/vanilla.yml)
- [npm](https://code.il2.dso.mil/tron/products/digitize/digitize-pipeline/-/blob/master/npm.yml)
- [yarn](https://code.il2.dso.mil/tron/products/digitize/digitize-pipeline/-/blob/master/yarn.yml)
- [mkdocs](https://code.il2.dso.mil/tron/products/digitize/digitize-pipeline/-/blob/master/mkdocs.yml)
- [hugo](https://code.il2.dso.mil/tron/products/digitize/digitize-pipeline/-/blob/master/hugo.yml)

> The available CI/CD pipeline config files are viewable on the Digitize repository [here](https://code.il2.dso.mil/tron/products/digitize/digitize-pipeline).

## Env Variables

The following environment variables are required for the CI/CD pipeline to run:

> Reference the [Digitize Tutorial](https://confluence.il2.dso.mil/display/puckboardhelp/Common+API%3A+How+to+use+your+Scratch+Space+in+a+Digitize+Application) for details and a more complete discussion of these environment variables.

- BUILD_OUTPUT_DIR => Required for npm, yarn, mkdocs, hugo
- DIGITIZE_DEPLOYMENT_TYPE => branch or tag
- DIGITIZE_REGISTRY_ACCESS_TOKEN => reference docs. This value is in not unique to your project and is listed in the docs.
- DIGITIZE_IL_TYPE => IL4, Digitize only supports IL4 deployment at this time.
- SKIP_E2E_TEST => Set to true to skip running e2e tests. They're broken right now.

## Protected Branches

This was the weird thing for me at first.

> Reference: [GitLab protected branch docs](https://docs.gitlab.com/ee/user/project/protected_branches.html).

The idea is that there are **two** types of pipelines. Full Staging/Deployment pipleines and a mini-pipeline for branches. The only difference is that the mini-pipeline does not push to staging or production.

The full pipeline can **only** be used on your `master` branch.

The mini pipelines will **only** run on protected branches.

This is why you need protected branches. Unprotected branches cannot run pipelines and cannot merge into `master`.

You can protect a specific branch, but that is cumbersome. GitLab lets you define a pattern with wildcard matching that will match the branch name and then protect that branch. For example a branch called `fix/fix-rendering-issue` would be protected with the pattern `fix/*`.

This forces a branch-naming convention because your branches must comply with these patterns otherwise they will not build or merge.

> **Recommendation**: Protect patterns `fix/*` and `feature/*` for your branches. This lets you work on new capabilities within the `feature` namespace and bug fixes within the `fix` namespace.

> **Recommendation**: Allow Maintainers to merge and Developers+Maintainers to push.

## React specific

You need to add a `homepage` field to your `package.json` file and set its value to `"."`. This is required for running the React App in a nested path.

**Congratulations**, your repository should be ready to begin building your site!

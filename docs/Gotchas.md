# Gotchas

match your GPG key exactly to the email address that you use to sign in to code.il2.dso.mil.
user.name
user.email

git config --local user.signingkey <your key>

## Node.js version

The build will run using Node.js v16.3.0. I ran into issues because I originally built my project locally using `npx create-react-app` while using `node18` and `React 18`. This got messy.

**Better Solution**: Clone the provided repo to your local machine and start from there. That is a version that will successfully build with the CI/CD pipeline.

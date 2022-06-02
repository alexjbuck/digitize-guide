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

Expect that you will adjust several CI/CD settings and add several environment variables. Precisely which you need to set will depend on what type of site you are building. The guide is designed for a React App, but you can use the following systems to build your site:

- [Vanilla JS/HTML/CSS](https://code.il2.dso.mil/tron/products/digitize/digitize-pipeline/-/blob/master/vanilla.yml)
- [npm](https://code.il2.dso.mil/tron/products/digitize/digitize-pipeline/-/blob/master/npm.yml)
- [yarn](https://code.il2.dso.mil/tron/products/digitize/digitize-pipeline/-/blob/master/yarn.yml)
- [mkdocs](https://code.il2.dso.mil/tron/products/digitize/digitize-pipeline/-/blob/master/mkdocs.yml)
- [hugo](https://code.il2.dso.mil/tron/products/digitize/digitize-pipeline/-/blob/master/hugo.yml)

> The available CI/CD pipeline config files are viewable on the Digitize repository [here](https://code.il2.dso.mil/tron/products/digitize/digitize-pipeline).
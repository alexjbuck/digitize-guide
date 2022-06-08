# Step 4: Get a Scratch Space assigned

Scratch space is a server-side key-value store that allows for data persistence. It is part of the Tron Common UI API. 

You need to post in the Tron/Arcade channel to get a Scratch Space assigned to you. You will be the `SCRATCH_ADMIN` for the scratch space.

TODO: Fill in more steps once I get this done for the first time.









When running `docker compose build` I ran into an error with 3 images:

1. registry.il2.dso.mil/platform-one/devops/pipeline-templates/base-image/harden-nginx-19:1.19.2
2. registry.il2.dso.mil/platform-one/devops/pipeline-templates/ironbank/nodejs16:16.3.0
3. registry.il2.dso.mil/platform-one/devops/pipeline-templates/ironbank/maven-jdk11:3.6.3

The way I solved this was to `docker pull <image>` for each of the three. Something in the `docker compose` was not pulling these dependencies. I think it might have had trouble getting the authentication for regular docker _and_ registry.il2.dso.mil.


`docker compose build` still fails with:

```
#0 73.35 [ERROR] Failed to execute goal org.apache.maven.plugins:maven-clean-plugin:3.1.0:clean (default-clean) on project common-api: Execution default-clean of goal org.apache.maven.plugins:maven-clean-plugin:3.1.0:clean failed: Plugin org.apache.maven.plugins:maven-clean-plugin:3.1.0 or one of its dependencies could not be resolved: The following artifacts could not be resolved: org.apache.maven:maven-model:jar:3.0, org.codehaus.plexus:plexus-utils:jar:2.0.4, org.apache.maven:maven-artifact:jar:3.0, org.sonatype.sisu:sisu-inject-plexus:jar:1.4.2, org.sonatype.sisu:sisu-inject-bean:jar:1.4.2, org.sonatype.sisu:sisu-guice:jar:noaop:2.1.7, org.apache.maven.shared:maven-shared-utils:jar:3.2.1, commons-io:commons-io:jar:2.5: Could not transfer artifact org.apache.maven:maven-model:jar:3.0 from/to central (https://repo.maven.apache.org/maven2): Transfer failed for https://repo.maven.apache.org/maven2/org/apache/maven/maven-model/3.0/maven-model-3.0.jar: Unknown host repo.maven.apache.org: Name or service not known -> [Help 1]
...
...
failed to solve: executor failed running [/bin/sh -c mvn clean]: exit code: 1
```
Those components are from the `backend` component from `docker-compose.yml`. So, I just ran `docker compose build backend` to run that stage in isolation and it worked! __I have not explored why this difference occurs__.

After running `docker compose build` and then `docker compose build backend` I believe that `docker compose up -d` will now work.
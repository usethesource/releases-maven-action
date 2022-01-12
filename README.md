# UseTheSource's custom release Maven artifacts

A UseTheSource specific action that takes care of deploying to the UseTheSource release server (<https://releases.usethesource.io/maven/>).

It takes care of performing the `mvn deploy` itself.

**Note: this won't work for SNAPSHOT releases**

## Using this action

Add the following section to your github deployment:

```yaml
      - name: Deploy
        if: startsWith(github.ref, 'refs/tags/v') # only tagged releases starting with a v will run this action
        uses:  usethesource/releases-maven-action@v1 # this will run mvn deploy -DskipTests for you
        with:
          maven-username: ${{ secrets.RELEASE_MAVEN_USERNAME }} # the secretes are defined for all repo's in the usethesource organisation
          maven-password: ${{ secrets.RELEASE_MAVEN_PASSWORD }}
          maven-local-port: ${{ secrets.RELEASE_MAVEN_LOCAL_PORT }}
          ssh-hostname: ${{ secrets.RELEASE_SSH_SERVER }}
          ssh-known-host: ${{ secrets.RELEASE_SSH_KNOWN_HOSTS }}
          ssh-username: ${{ secrets.RELEASE_SSH_USERNAME }}
          ssh-private-key: ${{ secrets.RELEASE_SSH_PRIVATE_KEY }}
          maven-options: "-DoptionalFlags -DyouWantToPass"
          working-directory: "./subdirectory"
```

Note that the if condition doesn't support regex. If you want to be more restrictive, you can add the following guards at the top of your action:

```yaml
on:
  push:
    branches:
      - main
    tags:
      - 'v[0-9]+.*'
    pull_request:
      branches:
      - main
```

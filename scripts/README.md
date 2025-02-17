# scripts

## add-users-to-team-from-list.sh

Invites users to a GitHub team from a list.

1. Create a new csv file with the users you want to add, 1 per line
2. Make sure to leave a trailing line at the end of the csv
3. Run: `./add-users-to-team-from-list.sh users.csv <org> <team>`

Example input file:

```csv
joshjohanning
FluffyCarlton

```

## ado_workitems_to_github_issues.ps1

Migrate work items from Azure DevOps to GitHub issues - this just links out to a [separate repo](https://github.com/joshjohanning/ado_workitems_to_github_issues)

## add-codeowners-file-to-repositories.sh

Adds a `CODEOWNERS` file to a list of repositories.

1. Run: `./generate-repos.sh <org> > repos.csv`
    - Or create a list of repos in a csv file, 1 per line, with a trailing empty line at the end of the file
2. Run: `./add-codeowners-file-to-repositories.sh repos.csv ./CODEOWNERS false`
    - For the 3rd argument, pass `true` if you want to overwrite existing file, otherwise it appends to existing

> **Note** 
> This is currently only checking for CODEOWNERS files in the root

## add-gitignore-file-to-repositories.sh

Adds a `.gitignore` file to a list of repositories.

1. Run: `./generate-repos.sh <org> > repos.csv`
    - Or create a list of repos in a csv file, 1 per line, with a trailing empty line at the end of the file
2. Run: `./add-gitignore-file-to-repositories.sh repos.csv ./.gitignore false`
    - For the 3rd argument, pass `true` if you want to overwrite existing file, otherwise it appends to existing

## create-enterprise-organizations.sh

Loops through a list of orgs and creates them.

## create-teams-from-list.sh

Loops through a list of teams and creates them.

1. Create a list of teams in a csv file, 1 per line, with a trailing empty line at the end of the file
    - Child teams should have a slash in the name, e.g. `test1-team/test1-1-team`
    - Build out the parent structure in the input file before creating the child teams; e.g. have the `test1-team` come before `test1-team/test1-1-team` in the file
2. Run: `./create-teams-from-list.sh teams.csv <org>`

Example input file:

```csv
test11-team
test22-team
test11-team/test11111-team
test11-team/test11111-team/textxxx-team

```

## delete-branch-protection-rules.ps1

Delete branch protection rules programmatically based on a pattern.

## delete-repos-from-list.sh

Deletes a list of repositories.

1. Run: `./generate-repos-list.sh <org> > repos.csv`
2. Clean up the `repos.csv` file and remove the repos you **don't want to delete**
3. Run `./delete-repos-from-list.sh repos.csv`
4. If you need to restore, [you have 90 days to restore](https://docs.github.com/en/repositories/creating-and-managing-repositories/restoring-a-deleted-repository)

## delete-teams-from-list.sh

Loops through a list of teams and deletes them.

1. Create a list of teams in a csv file, 1 per line, with a trailing empty line at the end of the file
    - Child teams should have a slash in the name, e.g. `test1-team/test1-1-team`
    - `!!! Important !!!` Note that if a team has child teams, all of the child teams will be deleted as well
2. Run: `./delete-teams-from-list.sh teams.csv <org>`

Example input file:

```csv
test11-team
test22-team
test11-team/test11111-team
test11-team/test11111-team/textxxx-team

```

## gei-clean-up-azure-storage-account.sh

Clean up Azure Storage Account Containers from GEI migrations.

## generate-repos-list.sh

Generates a list of repos in the organization - has many uses, but the exported repos can be used in the `delete-repos-from-list.sh` script.

Credits to @tspascoal from this repo: https://github.com/tspascoal/dependabot-alerts-helper

1. Run: `./generate-repos.sh <org> > repos.csv`

## generate-users-from-team.sh

Generates a list of users from a team in the organization - has many uses, but the exported users can be used in the `remove-users-from-org.sh` script.

1. Run: `./generate-users-from-team <org> <team> > users.csv`

## get-app-jwt.py

This script will generate a JWT for a GitHub App. It will use the private key and app ID from the GitHub App's settings page.

1. Run: `python3 get-jwt.py ./<private-key>.pem <app-id>`
    - You can also just run `python3 get-jwt.py` and it will prompt you for the private key and app ID
    - You will need to have the `jwt` package installed via `pip3`: `pip3 install jwt`
    - The JWT is valid for 10 minutes (maximum)

Script sourced from [GitHub docs](https://docs.github.com/en/apps/creating-github-apps/authenticating-with-a-github-app/generating-a-json-web-token-jwt-for-a-github-app#example-using-python-to-generate-a-jwt).

## get-app-tokens-for-each-installation.sh

This script will generate generate a JWT for a GitHub app and use that JWT to generate installation tokens for each org installation. The installation tokens, returned as `ghs_abc`, can then be used for normal API calls. It will use the private key and app ID from the GitHub App's settings page and the `get-app-jwt.py` script to generate the JWT, and then use the JWT to generate the installation tokens for each org installation.

1. Run: `./get-app-tokens-for-each-installation.sh <app_id> <private_key_path>"`
    - Requires `python3` and `pip3 install jwt` to run the `get-jwt.py` script to generate the JWT
    - The installation access token will expire after 1 hour

Output example:

> Getting installation token for: Josh-Test ...
> 
>  ... token: ghs_abc
> 
> Getting installation token for: joshjohanning-org ...
> 
>  ... token: ghs_xyz

Docs:
- [Generate a JWT for a GitHub App](https://docs.github.com/en/apps/creating-github-apps/authenticating-with-a-github-app/generating-a-json-web-token-jwt-for-a-github-app#example-using-python-to-generate-a-jwt)
- [Generating an installation access token for a GitHub App](https://docs.github.com/en/apps/creating-github-apps/authenticating-with-a-github-app/generating-an-installation-access-token-for-a-github-app#generating-an-installation-access-token)
- [List installations for the authenticated app](https://docs.github.com/en/rest/apps/apps?apiVersion=2022-11-28#list-installations-for-the-authenticated-app)

## get-new-outside-collaborators-added-to-repository.sh

This script will generate a list of new outside collaborators added to a repository. It uses a database file specified to determine if any new users were added to the repository and echo them to the console for review.

My use case is to use this list to determine who needs to be added to a organization's project board (ProjectsV2).

1. Run: `./new-users-to-add-to-project.sh <org> <repo> <file>`
2. Don't delete the `<file>` as it functions as your user database

## migrate-npm-packages-between-github-instances.sh

Migrate NPM packages in GitHub Packages from one GitHub organization to another.

1. Define the source GitHub PAT env var: export GH_SOURCE_PAT=ghp_abc (must have at least `read:packages`, `read:org` scope)
2. Define the target GitHub PAT env var: export GH_TARGET_PAT=ghp_abc (must have at least `write:packages`, `read:org` scope)
3. Run: `./migrate-npm-packages-between-github-instances.sh joshjohanning-org github.com joshjohanning-emu github.com`

## remove-users-from-org.sh

Removes a list of users from the organization.

1. Create a list of users in a csv file, 1 per line, with a trailing empty line at the end of the file (or use `./generate-users-from-team <org> <team>`)
2. Run: `./remove-users-from-org.sh <file> <org>`

## update-repo-visibility-from-server-to-cloud.ps1

Compares the repo visibility of a repo in GitHub Enterprise Server and update the visibility in GitHub Enterprise Cloud. This is useful since migrated repos are always brought into cloud as private.

## verify-team-membership.sh

Simple script to verify that a user is a member of a team

# 🚧🏗 THIS IS WORK IN PROGRESS

# rename-github-branch

> CLI to rename a branch on GitHub repositories

## Usage

You need to create a personal access token with the "repo" scope:
https://github.com/settings/tokens/new?scopes=repo

Replace `<ACCESS TOKEN>` below with the token you just created.

Rename branch for specific repository

```
npx rename-github-branch --token=<ACCESS TOKEN> --repo=gr2m/rename-github-branch
```

Rename branch for all repositories in an organization or account

```
npx rename-github-branch --token=<ACCESS TOKEN> --owner=gr2m
```

## How it works

1. Creates a new reference using the `sha` of the last commit of `current_branch`
   ([`POST /repos/:owner/:repo/git/refs`](https://developer.github.com/v3/git/refs/#create-a-reference))
2. Updates the default branch of the repository ([`PATCH /repos/:owner/:repo`](https://developer.github.com/v3/repos/#edit))
3. Updates branch protection to the new branch name if applicable ([GraphQL mutation `updateBranchProtectionRule`](https://developer.github.com/v4/mutation/updatebranchprotectionrule/))
4. Look for open pull requests and update the base branch if it’s `current_branch` ([`PATCH /repos/:owner/:repo/pulls/:number`](https://developer.github.com/v3/pulls/#update-a-pull-request))
5. Delete `current_branch` ([`DELETE /repos/:owner/:repo/git/refs/:ref`](https://developer.github.com/v3/git/refs/#delete-a-reference))

## Motivation

I think `master` is a horrific term. `git` & GitHub should follow the lead of
tech communities such as [Django](https://github.com/django/django/pull/2692),
[Drupal](https://www.drupal.org/project/drupal/issues/2275877) and
[CouchDB](https://issues.apache.org/jira/browse/COUCHDB-2248) and replace
`master` with non-offensive term, such as `latest`. This library is meant to
simplify the process of renaming branch names of GitHub repositories.

## License

[MIT](LICENSE)

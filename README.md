# Ruleset recipes
Starter rulesets are pre-baked to make it easy to get started with [repository rules](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets).

## What is a ruleset?

A ruleset is a named list of rules that applies to a repository. You can create rulesets to control how people interact with selected branches and tags in a repository. You can control things like who can push commits to a particular branch or who can delete or rename a tag. For example, you could set up a ruleset for your repository's feature branch that requires signed commits and blocks force pushes for all users except repository administrators.

## Get cooking
1. Grab a copy of this repo
 - ⬆️ top click `< > Code`
 - Pick your favorite way to clone, like [GitHub CLI](https://cli.github.com/), or download the ZIP.    
1. To get started, visit your favorite repository or organization you have admin access to.
 - Head to ⚙️Settings > Rules > Rulesets
 - Select New Ruleset > Import a ruleset
 - Browse to your local clone of the ruleset-recipes you want to import
 - Review the imported ruleset and save your changes!
 - Success! 🎉
 - 
### Video Example
![Gif walking through the steps outline above to import a ruleset from a JSON file.](https://github.com/github/release-assets/assets/7575792/6160a4c1-f329-4b7e-8699-f0ba74be39ff)

# Table of contents
## Branch Rulesets
- [Branch protection best practices](https://github.com/github/ruleset-recipes/blob/a1f8e53ec12857637e8762e689a3abc255ff2c2f/branch-rulesets/were-just-normal-repositories.json)
- [Require Pull Requests and conventional commits](https://github.com/github/ruleset-recipes/blob/8cd19a8e06e6e523fffd43e4a59a554c210dcbe2/branch-rulesets/PRs%20and%20commits.json)
- [Organziation ruleset: One Rule to rule them all](https://github.com/github/ruleset-recipes/blob/8cd19a8e06e6e523fffd43e4a59a554c210dcbe2/branch-rulesets/org-rulesets/one-ruleset-to-rule-them-all.json)
## Tag Rulesets
- [Prevent Tag Deletions](https://github.com/github/ruleset-recipes/blob/a1f8e53ec12857637e8762e689a3abc255ff2c2f/tag-rulesets/prevent-tag-delete.json)
- [Organization ruleset: requiring semantic versioning and prevents deletion for all tags](https://github.com/github/ruleset-recipes/blob/ac4b5ebc05219bb07de10f6094ad9ae8215bd39c/tag-rulesets/org-ruleset/tag-defaults.json)

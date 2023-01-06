# Learn GitHub Actions

> The steps in this README are intended to introduce you to the basic mechanics of GitHub Actions.
> This guide is somewhat oriented toward the lens of a software developer, but it should be adaptable to other technical disciplines.

## Using this repository

1. You must have a GitHub account to follow this guide. Create one now if you do not have a GitHub account.
2. Use the "Fork" button in the top right to copy this repository into your own account for modification.
2. GitHub Actions uses the following structure for workflow files: Workflow > Job > Step > Action
![GitHub Actions file structure](./assets/github-actions-file-structure.png). From [GitHub's template repo](https://github.com/skills/continuous-integration):

   - **Workflow**: A workflow is a unit of automation from start to finish, including the definition of what triggers the automation, what environment or other aspects should be taken account during the automation, and what should happen as a result of the trigger.
   - **Job**: A job is a section of the workflow, and is made up of one or more steps. In this section of our workflow, the template defines the steps that make up the `build` job.
   - **Step**: A step represents one _effect_ of the automation. A step could be defined as a GitHub Action, or another unit, like printing something to the console.
   - **Action**: An action is a piece of automation written in a way that is compatible with workflows. Actions can be written by GitHub, by the open source community, or you can write them yourself!

## Workshop guide

This guide will help you create a workflow in your repository to demonstrate some GitHub Actions features.

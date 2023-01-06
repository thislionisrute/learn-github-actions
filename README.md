# Learn GitHub Actions

> The steps in this README are intended to introduce you to the basic mechanics of GitHub Actions.
> This guide is somewhat oriented toward the lens of a software developer, but it should be adaptable to other technical disciplines.

## Using this repository

1. You must have a GitHub account to follow this guide. Create one now if you do not have a GitHub account.
2. Use the "Fork" button in the top right to copy this repository into your own account for modification.
2. GitHub Actions uses the following structure for workflow files: Workflow > Job > Step > Action
![GitHub Actions file structure](./assets/github-actions-file-structure.png)
From [GitHub's template repo](https://github.com/skills/continuous-integration):

   - **Workflow**: A workflow is a unit of automation from start to finish, including the definition of what triggers the automation, what environment or other aspects should be taken account during the automation, and what should happen as a result of the trigger.
   - **Job**: A job is a section of the workflow, and is made up of one or more steps. In this section of our workflow, the template defines the steps that make up the `build` job.
   - **Step**: A step represents one _effect_ of the automation. A step could be defined as a GitHub Action, or another unit, like printing something to the console.
   - **Action**: An action is a piece of automation written in a way that is compatible with workflows. Actions can be written by GitHub, by the open source community, or you can write them yourself!

## Workshop guide

This guide will help you create a workflow in your repository to demonstrate some GitHub Actions features.

1. Note that if you click on the "Actions" tab now at the top of the repository, GitHub will suggest starter workflows with common steps and actions. This guide does not leverage any of these in order to help you gain more familiarity with the file structure, but they are a good place to begin a new workflow.
2. For this workshop, start by creating the directory structure and file at the following path: `.github/workflows/my-workflow.yml`. All GitHub Actions workflows are contained here. You may have multiple workflows for different needs.
3. Inside the `my-workflow.yml` file, add the following content to name the pipeline, and define its triggers:
    ```yml
    name: My first workflow
    on:
      push:
      branches: main
    ```
    This tells GitHub Actions to run the workflow anytime a commit is pushed to the repository.
    There are lots of different events within GitHub you can use to trigger your workflow.
4. Every workflow must have at least one "Job". A Job contains steps that execute Actions.
    An individual step may be a shell script or an available Action that codifies some other activity.
    Add the following job definition to your workflow:
    ```yml
    jobs:
      info:
        runs-on: ubuntu-22.04
        steps:
          - run: echo "$GITHUB_ACTOR is executing a workflow"
    ```
    This defines a job identified as `info` that runs on an Ubuntu runner (think of this as a blank Virtual Machine with some special GitHub software installed) that executes the shell script in the file.
5. Commit your `my-workflow.yml` file and push it to your forked repository.
    This should trigger your workflow to run.
6. In GitHub, navigate to your Actions, find your workflow, and drill in to view the logs.
    Can you find your name in the log output?
    This is because the script in the workflow uses a built-in environment variable to detect who triggered the workflow.
    Explore other pre-defined environment variables and define your own as needed to configure different actions and add context.

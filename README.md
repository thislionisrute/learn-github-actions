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
    This tells GitHub Actions to run the workflow anytime a commit is pushed to the `main` branch of the repository.
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
7. Up to this point, you've just used GitHub Actions to execute some code inside their service.
    More often, you need to check out your code repository onto the runner in order to do something with it.
    Add the following job after the `info` job in your workflow:
    ```yml
      build:
        runs-on: ubuntu-22.04
        steps:
          - uses: actions/checkout@v2
    ```
    The `checkout` action clones your repo on the GitHub runner.
8. This job doesn't really do anything of value yet. Next, you need to ensure you have the right tooling and correct versions installed on the runner. Add this step to the `build` job in your workflow:
    ```yml
          - uses: actions/setup-node@v2
            with:
              node-version: 16
    ```
    For many languages, there is setup action available to install the version of the SDK that you need to build your code.
    Notice the `with` property in the Yaml. This is where you provide input parameters to actions. Any action that allows configuration of its behavior will require parameters to be set in the `with` property.
9. Now, the workflow ensures that Node.js is installed so it can be used in the rest of that Job.
    Add the following steps to the `build` job to install dependencies and execute a build:
    ```yml
          - run: npm ci
          - run: npm run build
    ```
    If you're not familiar with Node.js and NPM, these commands just ensure that dependencies of this project are installed, and the `npm run build` command is triggers a custom script to build this project (a simple static website).
10. Commit the changes to your `my-workflow.yml` file so far and push to GitHub.
    In a few moments, you should be able to see a new run in your repository's Actions tab that performs the additional steps defined.
    Note on the workflow run page that the `info` and `build` jobs run in parallel. When you need to define dependencies between jobs, use the `needs` parameter in the job that depends on another.
    Troubleshoot any issues by looking at the logs and ensuring that the syntax of the workflow file is correct.
11. Now that the project builds successfully, you need to store a copy of the built website so that it can be deployed (manually _or_ by another job or workflow). Add the following step to your `build` job:
    ```yml
          - uses: actions/upload-artifact@v1
            with:
              name: website
              path: _site
    ```
    Note again the use of the `uses` and `with` properties to invoke a public GitHub Action and provide parameters to it.
    In this case, `name` defines the name of the artifact you are saving, and `path` defines where GitHub should find the artifact on the runner.
    This is evaluated relative to the project root.
12. Commit this final change to your `my-workflow.yml` files and push it to your GitHub repository.
    On the next run of your workflow, you should see that an artifact is created and made available at the bottom of the web page for the run.
    This can be downloaded and inspected, or used by another job or workflow to be deployed or added to an artifact repository or file share.

## Next steps

Congratulations! 🎉 You now have a simple but functional GitHub Actions workflow to build and archive this website. From here, look into the [GitHub documentation](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions) or the [Microsoft Learn site](https://learn.microsoft.com/en-us/training/paths/automate-workflow-github-actions/) for more in-depth guides on GitHub Actions to learn about other features. Explore the [GitHub Actions Marketplace](https://github.com/marketplace/actions/) to find publicly listed Actions you can incorporate into your projects.

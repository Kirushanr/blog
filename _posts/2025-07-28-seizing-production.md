---
title: 'Seizing production: a deployment chaos'
category: devsecops
---

![Seizing Production Cover](https://gist.github.com/user-attachments/assets/e708621b-ca16-48c7-85cb-ca98776ce18e)

## Synopsis

We are going to look at a very simple CI/CD insecurity, that allows anyone who have write privileges  to a deployment pipeline in your organisation, to create chaos in a production environment. We will cover, how this can be accomplished in Azure Devops, GitHub and Bitbucket and how we can mitigate this in simple steps.


## CI/CD for layman

Most people use the  word  **CI/CD** as a colloquial term to tell the non-technical folks like me, that they are shipping code using a service, which builds and deploys their software to someone's computer (aka Cloud/Server running in a basement).

![Example of Simple CI/CD flow](https://gist.github.com/user-attachments/assets/8bbdf6e4-15df-483b-b7d6-fbdc947181a7)

As shown in the diagram above, 
* A developer writes the code, pushes it to a remote repository
* Based on the pipeline definition (we are assuming that the repository has one) it will build the code and a build artifact gets generated (e.g. Docker image)
* The build artifact then gets deployed to an intended destination (e.g. a server)

Looks so simple isn't it !,  wait until your customers hears about this !.


## Kenevil enters the pipeline

![For the better right meme used an example to illustrate organisations that do not have branch protections](https://gist.github.com/user-attachments/assets/a8d5e416-955b-4095-b745-a75c88395d5e)

There is no CI/CD security writeups without branch protection. It is the fundamental defence in protecting your source code from unintended modification.  Branch protection is a simple set of rules that enforces who can do what to the source code in a branch.

I am not going to deep dive into the branching strategies, let's keep it simple and say we use **main** as our production branch. A branch that contains production code should have at least the following rules:
* No one should be able to edit/modify/write anything to the **main** branch directly (e.g. Force push)
* Any PRs to the production branch should at least be reviewed by two people  (four-eyes principle) excluding the person who is making the change. Everything else can be tweaked and modified according to your specific situation

All major source code providers have great literature on how this can be accomplished :
* [GitHub](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/managing-a-branch-protection-rule)
* [Azure Repos ](https://learn.microsoft.com/en-us/azure/devops/repos/git/branch-policies?view=azure-devops)
* [BitBucket](https://support.atlassian.com/bitbucket-cloud/docs/use-branch-permissions/)

Now we pat ourselves in the back for the job well done ... , but I want to disappoint you and say that someone with the right intentions can still ship code to production bypassing the branch protection. 

![Diagram illustrating deployment environment is unprotected](https://gist.github.com/user-attachments/assets/83b1fb43-2847-4c84-bc25-bd7803a8c541)

As I highlight in the above diagram, most pipelines **let this wide open**, anyone can create a branch and trigger the pipeline to build and deploy a build artifact to a production environment. For example, a malicious actor can create an evil branch and sneakily deploy a malicious version of an artifact to end users.  

In the next sections, we will look at how this insecurity exists  on all major services we mentioned eariler.

### Github action

Now in GitHub actions, the environment does not set restrictions on which branch can deploy, see the the example image shown below.  

![Insecure default configuration of environment in GitHub Action](https://gist.github.com/user-attachments/assets/e762791a-da38-4739-b125-8978b2fd5020)

Since, we have branch protection, we should set this setting to **selected branches and tags** and restrict it to that branch. Ensure that you are using environments feature with this setting enabled. 
You can read [GitHub's official documentation](https://docs.github.com/en/actions/how-tos/deploy/configure-and-manage-deployments/manage-environments).


### Azure DevOps

In Azure DevOps similar insecurity exists, this can occur in an Azure DevOps environment or Agent Pool . This totally depends on how someone is deploying and where they are deploying. Creating an environment provisions the Agent pool with the restrictions, but if you are creating an Agent pool directly, then it has to be enforced there.

In an Azure DevOps environment, this setting is hiding under Approval and checks tab and called Branch control.
![Azure DevOps environment settings, branch control](https://gist.github.com/user-attachments/assets/e1f423ea-58d2-4391-b32f-fdd30244caad)

In an agent pool, these settings are hiding under the same spot. Now we need to ensure this branch is set to our protected branch, e.g. **main**.

![Azure DevOps Agent Pool, branch control](https://gist.github.com/user-attachments/assets/c2462757-d635-459c-a6fe-746e03c5afed)

[You can read Azure DevOps official documentation](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/approvals?view=azure-devops&tabs=check-pass)


### Bitbucket 

On Atlassian Bitbucket, you must pay the **Security Tax** to unlock this feature, It can only be enabled in Premium plan of Bitbucket and you must use deployments.

![Atlassian Deployment Permissions](https://gist.github.com/user-attachments/assets/6f5d9360-7c15-451a-9c37-0b70997eb941)

[You can read the Atlassian release documentation here ](https://www.atlassian.com/blog/bitbucket/build-trust-in-your-deployment-workflow-with-deployment-permissions)


## Epilogue



> Attack him where he is unprepared, appear where you are not expected - Sun Tzu 
   (Art of War : Chapter 1  - Laying plans)

Most CI/CD pipelines are insecure by default, I suggest that you take a look at [OWASP CI/CD Security Cheat sheet ](https://cheatsheetseries.owasp.org/cheatsheets/CI_CD_Security_Cheat_Sheet.html) to  harden your environment based on your workflows and business requirements.

Thank you for reading this post, hopefully you learnt something today :).
I will see you in another 4 years, in my next post ....  

![skeletor-running-away](https://gist.github.com/user-attachments/assets/5d569b75-da5b-46f5-b3ee-82c155b658ea)

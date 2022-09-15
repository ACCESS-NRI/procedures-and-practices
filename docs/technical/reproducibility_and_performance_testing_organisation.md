Introduction

Official GitHub repositories need to be public for maximum visibility to
the ACCESS Community. Reproducibility and performance testing will have
to be run offsite on [self-hosted runners on HPC
hardware](https://docs.github.com/en/actions/hosting-your-own-runners/adding-self-hosted-runners)
at NCI.

GitHub [recommends not using self-hosted on public
repositories](https://docs.github.com/en/actions/hosting-your-own-runners/about-self-hosted-runners#self-hosted-runner-security):

> We recommend that you only use self-hosted runners with private
> repositories. This is because forks of your repository can potentially
> run dangerous code on your self-hosted runner machine by creating a
> pull request that executes the code in a workflow.

The CI needs to be configured to mitigate the security issues with
self-hosted runners on public repositories to an acceptable risk level.

What are the major risks?

-   Threat injection into software affecting users
-   Disruption to activities and wasted time and energy assessing damage
-   Reputational damage
-   Disruption/deletion of material at NCI
-   Compromise of NCI accounts

## Threat Identification

As noted above the major threat vector is a pull request (PR) from a
fork of a public repository which contains malicious code.

A major threat, with low likelihood, is a GitHub account of an
Organisation admin or maintainer being compromised.

A minor threat is an inadvertent change to CI configuration from a
trusted user or source that enables a major threat.

## Threat Mitigation

**Summary**

| **Action**    | **Platform**     | **Comments** |
|---------------|-----------------|----------------|
| 2FA  Authentication  | GitHub  | Admins and maintainers of ACCESS-NRI repos  |
| Admin         | GitHub      | Repo admin rights restricted to team lead  and above IF REQUIRED |
| Maintainer    | GitHub | Repo maintainer rights restricted to team  members IF REQUIRED |
| GITHUB_TOKEN  | GitHub | Reduce default permissions    |
| Open dev      | GitHub | Public PR to dev branch only  |
| Protected main | GitHub | PR to main only from members of designated team |
|                |        | Require 2 reviews                           |
|               |        | Only signed commits                         |
|               |        | Do not allow bypassing branch protection settings |
| GitHub  environment | GitHub | Require designated team approval for NCI CI tests |
| Reusable  workflows | GitHub | Less error prone                            |
|               |       | Cleaner                                     |
|               |       | Isolates changes to CI in more secure repo  |
| Service user  | NCI   | Run jobs as service user. Reduces attack surface, severity of attack and account compromise |
| JobFS         | NCI   | Run job entirely in ephemeral JobFS space on compute nodes. Reduces possibility of harmful artefacts. |
| Pre-run checks | NCI   | Have independent script run pre-run checks before jobs submitted. |
| Containers    | NCI   | Run entirely inside singularity containers. |
|               |       | Massively reduce attack surface with read-only mounts |
| Audit         | NCI   | Run post-run audit to check for anomalous   behaviour

Apply the [principle of least
privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege).

## GitHub

Require all admins and maintainers of the ACCESS-NRI organisation or repositories to [enable two-factor authentication (2FA)](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication) to minimise threat of compromised accounts.

Team lead and above will have admin status **if required**. Everyone else maintainer status **if required**. Elevated privileges are a responsibility that requires work to understand the ramifications of actions. It should be a relief not to have to shoulder that burden.

Reduce default permissions for GITHUB_TOKEN to [prevent malicious use of API requests](https://securitylab.github.com/research/github-actions-building-blocks/).

GitHub defaults to not running CI on a PR from a first-time contributor and requires authorisation from a maintainer to run the workflow. This can be circumvented by [submitting an innocuous PR which is authorised for CI, and then push subsequent malicious code to the PR](https://blog.gitguardian.com/github-actions-security-cheat-sheet/) branch. It is also possible for a trusted contributor GitHub account to be compromised and start a PR with malicious code. So while these defaults are worthwhile they are not sufficient.

As it is insufficient to rely on default protections, offsite self-hosted runners must either only run workflows from specified protected branches or only run after approval from nominated team members (or both).

**Option A - Protected branch:** 

Allow public PR to dev branch which runs only GitHub CI compilation checks. CI with self-hosted runners is restricted to PRs to the main branch which is protected and only allows a PR from dev from designated team members.

Suggested [branch protection rules](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/defining-the-mergeability-of-pull-requests/about-protected-branches#restrict-who-can-push-to-matching-branches)
on main branch for security purposes (other branch protection rules are
appropriate but for code quality reasons):

-   Require pull request (no direct pushes)
-   Require 2 reviews before merging
-   Prevent force-push
-   Prevent branch deletion, or creation of matching branches
-   Allow only [signed commits](https://docs.github.com/en/authentication/managing-commit-signature-verification/about-commit-signature-verification)
-   Do not allow bypassing branch protection settings

Signed commits are important. It is [trivially easy to impersonate another user](https://betterprogramming.pub/why-and-how-you-should-sign-all-your-git-commits-94435516edae) when committing to git. This does lead to elevated privileges, but could be exploited in a social hack, causing commits to be accepted based on an assumption of identity.

A drawback of this approach is code gets committed to the dev branch without other checks, such as reproducibility. These checks would only be run on a subsequent PR to the main branch. This violates the scrum dictum to fix bugs as quickly as possible.

One way to address this (though it adds complexity) is to automatically create a PR to main when approvals pass, and [make the original PR dependent](https://github.com/marketplace/actions/dependent-issues) on the new PR passing. This would mean code gets added to main before dev, which is not intuitive.

**Option B -- Environment approval:** 

Define a [GitHub environment](https://docs.github.com/en/actions/deployment/targeting-different-environments/using-environments-for-deployment#deployment-branches) (e.g. "reproducibility") which requires specified team sign-off. Allow public PR to main branch which runs only GitHub CI compilation checks by default, and use environment: reproducibility in workflow specification for offsite (self-hosted) checks. Pull requests are run with the base repo of the PR, so the CI yaml can't be directly altered for the PR checks themselves. A weakness of this approach is that human error could miss malicious or unintentionally harmful modification that removes the environment specification so that subsequent PRs will run without sign-off.

**Option C -- Protected branch and environment approval:** 

Combine option A and B. Only allow PR to a dev branch, and use [GitHub environment](https://docs.github.com/en/actions/deployment/targeting-different-environments/using-environments-for-deployment#deployment-branches) to require approval for offsite (self-hosted) runs with PR from dev to main. This is the most secure option. It doesn't stop harmful changes to the CI, but there is an extra level of checks to navigate and a team member has to initiate the PR, so it can't happen automatically.

Option B is the simplest and easiest for Community members as it requires no work on their part. Option C is the most secure, but it does require pull requests to be made against a specific branch that is not the default branch. It is possible to [change the default branch](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-branches-in-your-repository/changing-the-default-branch), which is the default branch used for PRs, but it is also the default branch used for git clone and displayed on the web interface.

Reusable workflows are workflow yaml files that can be reused in repositories. Should strongly prefer to use [reusable workflows](https://docs.github.com/en/actions/using-workflows/reusing-workflows) in CI:

-   Reduces technical debt in CI (improvements automatically propagated
    to all CI)
-   Isolates major CI changes to a repository with much higher security
-   Less error prone

Possibility of running CI checks for changes to GitHub workflow configuration.

## NCI (Runners)

Run as a service user. Reduces the potential for harmful attack and inadvertent changes to a user environment which may introduce more attack possibilities. Also reduces the damage in the case of account compromise.

Run entirely in ephemeral PBS jobfs space. Reduces the potential for harmful or malicious artefacts to persist and be reused.

Use [automated pre-run script](https://docs.github.com/en/actions/hosting-your-own-runners/running-scripts-before-or-after-a-job) to make some consistency checks, e.g. job name matches allowed list.  This has the advantage of being completely independent of the repository code, and not visible to malicious actors. This also means the script needs to be kept in sync with tests, but this is a feature not a bug: can't just run new tests without some oversight. Could have a post-run script in the same way, which is an opportunity to run some security checks.

[Singularity containers can be used at NCI](https://opus.nci.org.au/display/Help/Singularity). Containers can reduce the attack surface for malicious code, particularly using user-bind to create read-only mounts.

Use [automated post-run script](https://docs.github.com/en/actions/hosting-your-own-runners/running-scripts-before-or-after-a-job) to do post-run audit.

Could reduce privileges of service user to prevent writing, or deleting, from important directory trees. This is problematic, as it is fragile and prone to inadvertent change. It should not be relied upon, so should probably not be used.

## Auditing

Log information on files generated, runtime and memory usage to spot anomalous activity.

Run cron workflows to check repo settings are correct.

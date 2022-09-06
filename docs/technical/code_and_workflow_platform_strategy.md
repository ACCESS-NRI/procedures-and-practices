# Code and Workflow Management Strategy
## Executive Summary

For code hosting the proposal is to use public GitHub repositories,
where code licensing allows, and ZenHub for workflow management. See
below for a summary of design criteria and comments on how these are met
with the chosen tools.

Note that the UM Atmosphere model has strict licensing conditions that
do not allow it to be hosted on publicly available services. It also
uses Subversion for version control. There were always going to be
issues with integrating the UM for these reasons, so the decision was
made to choose tools to be as open as possible and integrate as smoothly
as possible with existing communities and deal with the UM as a special
case.

Code hosting:

| **Criteria**                  | **Compliance**                       |
|-------------------------------|--------------------------------------|
| Publicly accessible           | Public GitHub repositories (where licensing allows) are  viewable without an account      |
| Low barrier to community participation | GitHub currently hosts all targeted open source model components |
| Version control software      | GitHub is based around git, the industry standard distributed version control system  |
| Code review tools             | Github pull requests (PRs) include support for code review, optionally this can be made mandatory |
| Community can report bugs/ask for changes | GitHub Issues                    |
| Community can suggest code changes   | GitHub pull requests support  code contributions from community |
| Continuous integration/testing  | GitHub workflows are highly configurable and tightly integrated with code review and  pull requests                    |

Workflow management:

| **Criteria**                   | **Compliance**                      |
|--------------------------------|-------------------------------------|
| Visible to the community       | ZenHub uses GitHub permissions, so read-only access will allow viewing the workflow and current progress |
|                                | Zenhub works directly with GitHub issues, still publicly visible |
|                                | Open source is free, so will scale seamlessly to however many community members need access |
| Organise work across multiple  repositories | Zenhub supports multirepository workspaces across organisations |
| Support agile workflows        | ZenHub is highly configurable and expressly supports agile development:  Sprints, Epics, Planning poker, Velocity estimation |
| Project planning and tracking functionality | ZenHub provides high level roadmaps, burndown charts and other tools for measuring productivity |
|                                | ZenHub provides dedicated release support, projects and epics for work planning |
| High level reporting tools     | ZenHub roadmap and release functionality |
| On time delivery               | ZenHub includes extensive support for agile development,  planning and reporting, enabling timely delivery |

## Introduction

ACCESS-NRI (NRI) has a requirement to develop climate, weather and
earth-system research models for the Australian community. These models
consist of complex scientific code bases for multiple model components
and associated documentation. Excellent software development practices
and powerful tools are required to properly manage such a complex
software ecosystem.

Community building is a foundational requirement of the NRI and an
important determinant in its success. Building a community requires high
levels of trust in the outputs of the NRI, and excellent communication
between the NRI and stakeholders. This leads to some fundamental guiding
principles:

-   Open and transparent
-   Responsive to community needs

Trust in the work of the NRI is essential. Trust can be engendered by
the following approaches:

-   Best practice software development
-   Well tested, validated and reproducible models
-   Full provenance of models and model output
-   Well supported products
-   On time release delivery

### Requirements for Code Development Tools

Best practice software development includes using modern version control
systems, continuous integration (CI) testing, code review and excellent
documentation. There is a hierarchy of testing and typically it is
compilation \> unit \> run \> reproducibility \> performance. Code
review is also an important element in producing robust model code that
the community can trust.

Reproducibility is essential. It provides a check on code change
correctness. If a model cannot reproduce answers this has to be
understood and documented. Users need to know if model changes lead to
changes in model answers in the case where they need to reproduce
earlier experiments.

Model performance, running time, memory use etc, need to be tracked.
Significant reductions in performance must be detected in automated
testing, investigated and remedied if possible. Where performance
reductions are unavoidable, they also need to be documented to enable
users to make informed decisions about which model version they need to
use for their work.

For version control, Git is preferred to SVN as it is the industry
standard. By choosing Git-based repositories, the developers can take
advantage of a plethora of development tools with Git integrations that
do not exist for SVN.

Full provenance for a released experiment means, for a given data
product being able to determine

-   exact source code version
-   build process, including versions of all supporting libraries
-   model inputs

Model inputs themselves require full provenance, including identifying
the original source data and all transformations required to produce the
files used in the model.

The goal is to be able to reproducibly build the model and re-run an
experiment and reproduce the earlier results. This means users can trust
they can always identify how their experiment was run and reproduce the
results when necessary.

Another goal is that users can take well documented NRI experiments and
create modified versions for examining their own scientific questions.
In this way the NRI can improve the productivity and impact of the
Australian research community.

NRI development activities must be responsive to community needs. The
code needs to be easily accessible to the community. Members of the
community must be able to lodge bug reports, feature requests and report
omissions in documentation. The process for doing so must be
straightforward and well understood.

A well-supported product must have excellent documentation and timely
resolution of errors, omissions or bugs.

The NRI is supporting community models, this means the community will
actively participate in the development of the models and software
curated by the NRI. As such, the code development tools must be
accessible by the whole community and easy to learn and operate.

### Requirements summary

| **Category**   | **Requirements**                  |
|----------------|-----------------------------------|
| Development    | Open to collaboration from the community |
|                | Open, transparent bug reporting   |
|                | Low barrier to community participation |
|                | Git version control if possible   |
| Testing        | CI compilation testing            |
|                | Run testing                       |
|                | Reproducibility testing           |
|                | Performance testing               |
| Provenance     | Code version control              |
| Code review    | Support for code review during update |

## Proposed Strategy

For git-based repositories, there are currently 3 different well-known
platforms: [Bitbucket](https://bitbucket.org), [Gitlab](https://gitlab.org) and [GitHub](https://github.com).

The proposal is to use GitHub for all open source code repositories.
GitHub is already the platform used by all the open source model
components the NRI intends to use in the initial tranche of releases.
Part of the community knows how to use it and there are well designed
online training courses available to all, which means a low barrier to
participate. As the code is already on GitHub there is the option to use
component code directly from the current repository. This is potentially
easier and meshes the NRI activities tightly with those of existing
communities. Even in the case where NRI creates a fork of an existing
repository the original and the fork are tightly coupled and it is
straightforward to synchronise changes between them.

GitHub issue support is excellent. It is easy to use, familiar to some
in the community, and offers many features that nicely tie issues
together, even across repositories and organisations.

Code updates are possible through pull requests, from either NRI staff,
or directly from the community. This process is well understood by part
of the community. Pull requests (PRs) can trigger continuous integration
(CI) testing, which can be configured so the code cannot be merged
without first passing CI testing. Pull requests can be connected to
issues and automatically close an issue when the PR code is merged.
Features like this help in streamlining code development. Code review is
supported in PRs, and can also be configured such that code cannot be
merged without first passing the code review.

Supporting the UM in this environment is complex. The code will have to
remain on the UKMO Subversion server. To integrate work on the UM as
closely as possible a UM repository will be created. It will not link to
code, but will contain issues so that work on the UM can be managed in
the same framework as the other model components. Community members can
also lodge their issues in this repository, removing the need to create
a UKMO account. We will use GitHub to release configurations for the UM
model component. These configuration repositories will contain all
information and auxiliary tools needed to build and run the
configuration.

### Strategy compliance

See Executive summary above.

Requirements for Workflow Management Tools

ACCESS-NRI operates in a complex software environment. Typically a model
will contain code from many separate repositories, in some cases those
repositories may not reside in the same location. The NRI will also have
to collaborate with existing model developers in the community. Flagship
releases will require collaboration between most NRI teams: model
development, model verification, training and release.

All input from the community will come in the form of issues and pull
requests in the relevant code repositories.

Consequently a workflow management tool must: be configurable to deal
with different workflow requirements, work across multiple repositories
in multiple organisations, and link very closely to the issues in the
code repositories.

Accurate and informative reporting tools, and transparent workflow
management allow the community to keep abreast of progress. Being open
with development progress gives the community confidence in release
timelines.

On time release delivery will show the community that the NRI is able to
follow through on commitments and can be a trusted partner in their
research program. Timely release in large complex software projects has
been proven to be a very difficult task with traditional project
management techniques. Agile software development is best suited to
accurately estimate and meet software delivery timelines.

## Requirements summary

| **Category**   | **Requirements**                  |
|----------------|-----------------------------------|
| Project management | Flexibility for different tasks |
|                    | Automated workflow tools |
|                    | Support different projects, releases and epic |
|                    | High level reporting tools, e.g. roadmaps |
| Issues and PRs     | Tight integration |
| Agile development  | Planning poker for timing estimates |
|                    | Sprint planning and execution     |
|                    | Velocity estimate (burndown charts etc) |
| Community          | On time delivery                  |
|                    | Open to the community             |

## Proposed Strategy

GitHub has a project management tool, called GitHub Projects, included
with GitHub repositories. However, this tool is quite simple and does
not work across more than one repository. As such, it is not suitable
for our purpose. Gitlab and Bitbucket come also with integrated project
management tools. These tools are very good, but they did not provide
enough of an advantage for us to choose these platforms.

The proposal strategy is to use [ZenHub](https://www.zenhub.com) for
code workflow management. ZenHub is built to work on top of GithHub,
either as a standalone web application, or as a plugin to the Chrome
browser working directly on the GitHub website. It uses GitHub issues
directly, and seamlessly integrates them into the ZenHub workflow. The
issues themselves are still visible on GitHub without using ZenHub.

ZenHub supports creating multiple workspaces with the same repositories.
Each workspace can be configured to give a different view of the
repositories, and with entirely different workflow setup. ZenHub
supports automatic workflows between workspaces. Thus ZenHub is highly
configurable and customisable for different tasks.

ZenHub supports different categories of task grouping: releases,
projects and epics. It has high level reporting tools showing the
progress of releases and projects.

ZenHub supports agile development and includes planning poker, sprint
planning and executtion, velocity estimates for better release timeline
planning and burndown charts for tracking spring progress.

### Strategy compliance

See [Executive summary](#executive-summary) above.

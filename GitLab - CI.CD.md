
_Some notes about general terminology and GitLab-specific terminology_

A _pipeline_ always refer to a _data pipeline_. It is a "linear DAG" of nodes (executors) and edges (pipes), where data flows from the start of the pipeline (the _source_ executor) to the end of it (the _sink_ executor). Executors other than the source and the sink are called _intermediate_. Each pipe is associated with a _data unit type_. Intermediate executors receive _data units_ from their _source pipe_, transform them and send them over their _sink pipe_. This generates a _stream_ of data units that flow through the pipe.

The best example of a data pipeline are the pipelines in the Unix shell. In the Go programming language it is possible to construct pipelines where, while still maintaining their "linearity", some sections of the pipeline are allowed to fan out from the pipeline (at the beginning of the section) and fan back into the pipeline (at the end of the section). This is possible because the pipes in Go are Go channels, which are more "sophisticated" than the good old Unix pipes.

A _workflow_, on the other hand, is a DAG of _tasks_. The tasks are executed in a sandbox and some sort of sharing must be in place in order for a task to use the generated products of their immediate predecessors.

Thus, there are some important differences between pipelines and workflows:

- Essential to the concept of pipelines is the fact that they enable a stream of data (units) that flows through the pipeline. In workflows there is no notion of "flow of data".
- Executors in a pipeline work in parallel, for as long as there is data flowing through them. On the other hand, a task in a workflow cannot start unless _all_ the tasks it depends on are _fully_ and successfully executed.
- A workflow can take the form of _any_ DAG, while a pipeline can only be linear (possibly with sections fanned out and back in).

Unfortunately in the GitLab documentation, the terms "pipeline" and "workflow" are both used, sometimes interchangeably, to refer to the workflows enabled by CI/CD. _I'll always use the term "workflow" in this context._

Additionally, in GitLab, workflow tasks are called _jobs_.

---

For a given project, workflows are specified in a single file that I'll call _the workflow file_. This file is, by convention, called `.gitlab-ci.yml` and located at the root of the project.

The workflow file is a "workflow cookbook" that:

- defines all the possible tasks that will be eventually executed
- contains instructions for "assembling" workflows out of these tasks (more precisely the instructions specify two things: which tasks are to be included in the workflow and in which order) depending of several factors like:
	- the triggering event(s)
	- the files changed (if applicable)
	- the state of the tree

GitLab offers the `rules` keyword to specify which tasks are to be included in a specific workflow execution.

GitLab offers two mechanisms to specify the order in which tasks are going to be executed:

- _Stages:_ define a very coarse ordering; tasks within a stage have no ordering and thus may be executed in any order.
- _The `needs` keyword:_ define a much finer ordering; it essentially specify the "dependency" edges of the workflow DAG.

Both mechanisms can be used in conjunction, in which case there are special rules for the resulting behavior, but in my opinion the `needs` keyword is more than enough, thus rendering stages completely irrelevant.

Workflows can be executed/triggered by various _events_:

- A tag is created (`$CI_COMMIT_TAG` is set).
- A branch is created (`$CI_COMMIT_BRANCH` is set).
- A _git push_ occured (`$CI_PIPELINE_SOURCE == "push"`). The pushed object might be:
	- A tag (`$CI_COMMIT_TAG` is set).
	- A collection of commits to a branch (`$CI_COMMIT_BRANCH` is set). Two cases are distinguished here:
		- The branch has an open merge request (`CI_OPEN_MERGE_REQUESTS` is set).
		- The branch has no open merge request (`CI_OPEN_MERGE_REQUESTS` is not set).
- A _gitlab merge request_ was created (`$CI_PIPELINE_SOURCE == "merge_request_event"`)



_Workflows_ are made up of _stages_ and _jobs_:

- Stages define the order of execution. Typical stages might be build, test, and deploy.
- Jobs specify the tasks to be performed in each stage. For example, a job can compile or test code.





_Runners_ are the processes that run _the GitLab Runner application_, to execute jobs in a pipeline.

_Runners_, from the perspective of a GitLab instance, are agents that are able to query the GitLab instance for available jobs to execute. From the implementation's perspective:

- A _runner manager_ is a process that runs _the GitLab Runner application_.
- Each runner manager can be configured with one or more `[[runners]]` sections, where each section defines a runner that will be registered into the GitLab instance. Each runner is configured with an authentication token, which is used to identify the runner to the GitLab instance.
- A runner manager has a pool of _workers_ (goroutines). A worker can "impersonate" a configured runner by using its authentication token. In such a case we can alternatively say that the worker is working _on behalf_ of the impersonated runner.
- Idle workers are periodically impersonating each configured runner (in a round-robin fashion) and asking the GitLab instance, on behalf of the impersonated runner, for available jobs to execute.

Each runner is configured with an _executor_, which determines how the jobs are actually going to be executed. There is a limited number of executor types (like `shell`, `ssh`, `docker`, `kubernetes` among others).

For a given project it is possible to configure a pool of runners in a hierarchical structure: some runners can be configured at the project level, group level or instance level.


---

We only consider two types of commits (we assume that commits with more than two parents never occur):

- _Standard commits_, with just one parent commit.
- _Merge commits_, with two parent commits: the the commit from the _main branch_ and the commit from the _affluent branch_.

_The trunk workflow_

We consider the _trunk workflow_, a very common _git workflow_ where:

- A main git branch called _trunk_ permanently exists.
- New git branches always branch from the trunk and are developed in parallel. I'll call such a git branch a _tranch_ (a trunk/topic branch). This is a new word I invented and should not be confused with the existing word _tranche_.
- Tranches are eventually merged back into the trunk or discarded altogether.

A typical software project workflow consists of three phases: _development_, _verification_ (build & test) and _deployment_. Each phase has its own workflow. One goal is to automate the verification and deployment workflows as much as possible.

In this context, the trunk workflow also requires that the trunk must always be deployable, i.e, it must always pass verification. This can be guaranteed by following the _trunk workflow discipline_.

_The trunk workflow discipline_

- Standard commits exist only in tranches.
- The main branch of a merge commit is always the trunk. The affluent branch of a merge commit is always a tranche.
- A tranch starts in _development mode_ and changes to _merge mode_ when a merge request for the tranch is created.
- If a tranch is in development mode, the verification workflow is always performed against the tranch itself (as usual). If the tranch is in merge mode, the verification workflow is always performed against the merge result.
- When a tranch gains new standard commits, the verification workflow is performed.
- A merge request can only be created if the corresponding tranch is verified. In such a case the tranch changes to merge mode and a verification workflow is performed again, but this time against the merge target.
- A merge request can only be approved if the verification against the merge target passes.
- If the merge request is approved, and the trunk has not changed since the last verification, then the merge is performed and a deployment workflow is performed using the result of the last verification. If the trunk has changed then a new verification workflow is started, but this time using the updated trunk.
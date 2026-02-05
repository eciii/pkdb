
**CI/CD**

_Pipelines_ are made up of _stages_ and _jobs_:

- Stages define the order of execution. Typical stages might be build, test, and deploy.
- Jobs specify the tasks to be performed in each stage. For example, a job can compile or test code.

Pipelines can be triggered by various _events_, like commits or merges, or can be on schedule.

For a given project, pipelines are defined/declared in a file, _the pipeline file_. This file is, by convention, called `.gitlab-ci.yml` and located at the root of the project.

_Runners_ are the processes that run _the GitLab Runner application_, to execute jobs in a pipeline.

_Runners_, from the perspective of a GitLab instance, are agents that are able to query the GitLab instance for available jobs to execute. From the implementation's perspective:

- A _runner manager_ is a process that runs _the GitLab Runner application_.
- Each runner manager can be configured with one or more `[[runners]]` sections, where each section defines a runner that will be registered into the GitLab instance. Each runner is configured with an authentication token, which is used to identify the runner to the GitLab instance.
- A runner manager has a pool of _workers_ (goroutines). A worker can "impersonate" a configured runner by using its authentication token. In such a case we can alternatively say that the worker is working _on behalf_ of the impersonated runner.
- Idle workers are periodically impersonating each configured runner (in a round-robin fashion) and asking the GitLab instance, on behalf of the impersonated runner, for available jobs to execute.

Each runner is configured with an _executor_, which determines how the jobs are actually going to be executed. There is a limited number of executor types (like `shell`, `ssh`, `docker`, `kubernetes` among others).

For a given project it is possible to configure a pool of runners in a hierarchical structure: some runners can be configured at the project level, group level or instance level.
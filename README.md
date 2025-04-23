## Workflows, jobs, and steps

📝 <i>Check `.github/workflows/01-building-blocks.yaml` for a proper example</i>

Creating a <i>workflow</i> needs to be in a `.github` 📁 folder with a `workflow` 📁 folder inside it.
<br />
This will be identified by `GitHub Actions` and handled by it.
<br />
You can then create a `.yaml` file inside the `workflows` folder

The `.yaml` file will include a `name` which will be the name of the <i>workflow</i> and after it is the `jobs` required property and `on` required property.

Under a <i>job</i>, we can define what runner or OS the job will run on using the `runs-on` property.

We need to define the `steps` the job will do. This `steps` is an array of scripts which the job will execute.

Each `steps` will have a `name` property,

```yaml
name: name-of-your-workflow
on: what-action-ie-push-pull-etc-check-gh-docu-for-valid-triggers
jobs:
  job-1-name:
    runs-on: os-or-runner-name
    steps:
      - name: i-am-a-step
        run: echo "I am a script"
      - name: i-am-another-step
        run: echo "I am another script"
  job-2-name:
    runs-on: os-or-runner-name
    steps:
      - name: i-am-a-step
        run: echo "I am another script from another job!"
```

Whenever a `steps` fails, the whole <i>workflow</i> is considered as FAILED. <b>Any subsequent steps will not execute.</b>

## Workflow Events

📝 <i>Check `.github/workflows/02-workflow-events.yaml` for a proper example</i>

There are several ways to trigger <i>workflows:</i>
<ul>
  <li><b>Repository events</b> - events that trigger workflows whenever there are changes or actions done on the repository</li>
  <li><b>Manual</b> - events that trigger workflows using the GH Actions UI, API calls, or from another workflow</li>
  <li><b>Scheduled</b> - Trigger workflows based on a set schedule, usually by a cron job</li>
</ul>

We can write events as array like so:

```yaml
name: workflow-name
on:
  - push
  - pull_request
```

Or we can write an event as a key. This way, we can customize it further by specifying which branch can trigger the workflow

```yaml
name: workflow-name
on:
  push:
  pull_request:
```

## Workflow Runners

We can think of <i>runners</i> as Virtual Servers or Virtual Machines that execute jobs from workflows.

Two types:
<ul>
<li>GitHub Hosted - managed service by GitHub; steps share the same VM, but jobs don't</li>
<li>Self Hosted - run workflows on almost any infrastructure of your choice</li>
</ul>

You can write the runner under `jobs` > `job-name` > `runs-on` property.
```yaml
name: workflow-name
on:
  push:
  pull_request:
jobs:
  job-name:
    runs-on: runner-name
    steps:
      - name: step-name
        run: echo "sample"
```

Keep in mind, if you have several jobs with different runners, the scripts may vary. Script from one runner may not be the same as a script from another runner.

We can add a `shell` property under a step to indicate which shell are you using to execute the scripts.

We can always check what softwares are included in a runner. GitHub has documentation.


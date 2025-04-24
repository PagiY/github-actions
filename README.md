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

Or we can write an event as a key. This way, we can customize it further by specifying which branch can trigger the workflow using <b>Event Filters</b>.

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

## Workflow Actions

📝 <i>Check `.github/workflows/04-using-actions.yaml` for a proper example</i>

Using actions instead of shell scripts.

An example would be setting up a node project. Instead of writing scripts to setup node, we can use github's pre-made actions for easy setup. This avoids repetitive and extensive commands.

An action can be configured via the `with` key-value pair - this enables great flexibility and reusability.

An action can be combined with other steps.

An action can be created for public or private use. Actions are not restricted to what's available in the marketplace. You can make your own action.

```yaml
name: workflow-name
on:
  push:
  pull_request:
jobs:
  job-name:
    runs-on: runner-name
    
    steps:
      - name: testing actions
        uses: action/checkout@v4
      - name: Setup node
        uses: actions/setup-node@v4
        with:
          node-version: '20.x'
```
## Event Filters

GitHub Actions has a feature that can specify under which conditions trigger workflow. 

Example:

```yaml
name: workflow-name
on:
  push:
    branches:
      - main
      - 'release/**'
    paths-ignore:
      - 'docs/**'
  pull_request:
jobs:
  job-name:
    runs-on: runner-name
    
    steps:
      - name: testing actions
        uses: action/checkout@v4
      - name: Setup node
        uses: actions/setup-node@v4
        with:
          node-version: '20.x'
```

In the example above, under we have two events: `push` and `pull_request`.

When a `push` event happens on the main branch or branches whose names match `release/**` pattern, trigger the workflow.

But when a `push` happens in the `docs/**` path, do not trigger the workflow. So we can ignore workflows from 
running whenever changes to a folder or file happens.

The workflow triggers for any `pull_request` event

## Activity Types

Specify which types of certain triggers execute the workflow. We can specify one or more activity types.

Example:

```yaml
name: workflow-name
on:
  push:
    branches:
      - main
      - 'release/**'
    paths-ignore:
      - 'docs/**'
  pull_request:
    types: [opened, synchronize]
    branches:
      - main
      - 'release/**'
jobs:
  job-name:
    runs-on: runner-name
    
    steps:
      - name: testing actions
        uses: action/checkout@v4
      - name: Setup node
        uses: actions/setup-node@v4
        with:
          node-version: '20.x'
```
The above example specified which type of `pull_request` triggers the workflow.
So whenever a `pull_request` is opened on `main` or branches on `release/**`, trigger the workflow.

## Workflow Contexts

Contexts offer many sources of information for our workflows.
Some contexts include information about runs, variables, jobs, and many more. 
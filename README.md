# listen.dev demos
PoC for lstn workflows in CI

### Intro

This repo demonstrates the CI use cases that `lstn` provides for JavaScript projects. It covers all core workflows currently supported ie.

1) Scanning npm dependencies in a project through lstn commands (using a `package.json` manifest)
2) Getting behavioral insights through verdicts
3) Automated policy enforcement in CI (using pre-defined expressions in `rules.yml`)


Currently there are two ways to utilize `lstn` in your CI process:

  1) Through the lstn CLI
  2) Through the lstn GitHub action


The CLI workflow handles everything on the user-side, purely depends on `lstn`, and is agnostic to the CI provider being used. All of the integration boilerplate is in [`./github/workflows/lstn-cli-workflow.yml`](https://github.com/garnet-org/demos/blob/main/.github/workflows/lstn-cli-workflow.yml).
The GitHub action workflow is provided through [`lstn-gh-action`](https://github.com/garnet-org/lstn-gh-action.git), which is a GitHub-native integration configurable through [`./github/workflows/lstn-docker-action-workflow.yml`](https://github.com/garnet-org/lstn-gh-action/blob/main/.github/workflows/lstn-docker-action-workflow.yml)


### How to invoke a scan:

- Right now, any `push` to this repo will invoke a scan. You can navigate to the Actions tab to view the execution logs for the action workflow. 
- Both CLI and action workflows read the `package.json` file present at this repository's root (also configurable through the `WORKDIR` variable). Making any changes to dependencies inside it will invoke the workflows.

### How to use rules:

- The [`rules.yml`](https://github.com/garnet-org/demos/blob/main/rules.yml) file contains a list of pre-defined `jq` expressions, which can be piped with lstn outputs to enforce policy. 
- Setting the `RULE_NAME` variable to a name from the list (e.g. `block_priority medium`) will enforce that rule.
- You can also `ignore` certain behaviors, which means that CI won't be halted even if that rule condition is met.

Some examples are:
 ```
  # Ignore medium priority alerts even if they pop up in CI

  - name: ignore_priority_medium
    query: .[] | select(.verdicts[]?.priority == "medium")
    behavior: ignore
    
  # Halt CI if any alert shows an outbound network connection

  - name: block_network_connection
    query: .[] | .verdicts[]? | select(.message == "unexpected outbound connection destination")
  ```

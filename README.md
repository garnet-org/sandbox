# listen.dev demos :dolphin:
> PoCs for lstn workflows

[![lstn](https://github.com/garnet-org/demos/actions/workflows/lstn.yml/badge.svg?branch=main)](https://github.com/garnet-org/demos/actions/workflows/lstn.yml)

### Intro

This repo demonstrates the use cases that `lstn` provides for JavaScript projects. It covers all core workflows currently supported ie.

1) Scanning npm dependencies in a project through lstn commands (using a `package.json` manifest)
2) Getting behavioral insights through verdicts
3) Automated policy enforcement in CI (using pre-defined expressions in `rules.yml`)


Currently there are two ways to utilize `lstn` in your CI process:

  1) Using the node action (`listendev/action@v0.2.0`)
  2) Using the Docker action (`garnet-org/lstn-policy@v0.0.3`)

### How to trigger the lstn workflows:

- As is, any `pull-request` event on this repo will invoke a scan. Simply create a PR with your desired changes in `package.json`, and view the PR [comment](https://github.com/garnet-org/demos/pull/10#issuecomment-1489536753) and execution logs for the [action](https://github.com/garnet-org/demos/actions).
- All workflows read the `package.json` file present at the repository's root (also configurable through the `workdir` option).

### How to use rules:

- The [`rules.yml`](https://github.com/garnet-org/demos/blob/main/rules.yml) file contains a list of pre-defined `jq` expressions, which can be piped with lstn outputs to enforce policy. 
- Setting the `rule-name` option to a name from the list (e.g. `block_priority medium`) will enforce that rule.
- You can also `ignore` certain behaviors, which means that CI won't be halted even if that rule condition is met.

Some examples are:
 ```
  # Ignore medium priority detections even if they pop up in CI

  - name: ignore_priority_medium
    query: .[] | select(.verdicts[]?.priority == "medium")
    behavior: ignore
    
  # Halt CI if any outbound network connection is detected

  - name: block_network_connection
    query: .[] | .verdicts[]? | select(.message == "unexpected outbound connection destination")
  ```

### Notes

`lstn` also supports CI workflows that don't use an action. In this case, the CLI works standalone--it handles all logic on the user-side, purely depends on `lstn`, and is agnostic to the CI provider being used. All of the integration boilerplate is in [`./github/workflows/lstn-cli-workflow.yml`](https://github.com/garnet-org/demos/blob/main/.github/workflows/lstn-cli-workflow.yml).

### Documentation

For detailed docs, see [docs.listen.dev](https://docs.listen.dev/)

# listen.dev demos 
[![lstn](https://github.com/garnet-org/demos/actions/workflows/lstn.yml/badge.svg?branch=main)](https://github.com/garnet-org/demos/actions/workflows/lstn.yml)
[![Release Notes](https://img.shields.io/github/release/listendev/action)](https://github.com/listendev/action/releases)
[![](https://dcbadge.vercel.app/api/server/Tmavx64a?compact=true&style=flat)](https://discord.gg/Tmavx64a)

> Understand your dependencies through behavioral monitoring & prevent supply chain attacks before they impact you. ⚡

## :dolphin: Intro

This repo demonstrates an end-to-end workflow for using `lstn` with JS/TS projects ie. 

1) Scanning a project's dependencies automatically at every change
2) Getting results ([verdicts](https://docs.listen.dev/concepts/verdicts)) inside dev workflows  
3) Customizing alerts and defining rule-based policy controls ([preview](https://docs.listen.dev/lstn-github-action/ignoring-results))

It uses the [action](https://github.com/marketplace/actions/scan-your-dependencies-with-the-listen-dev-cli) which is recommended for GitHub-based CI workflows. However, lstn can be integrated with [any CI system](https://docs.listen.dev/lstn-cli/integration-guides/ci) through the CLI (see [example workflow](https://github.com/garnet-org/demos/blob/main/.github/workflows/lstn-cli-workflow.yml)).

## 🪜 Step-by-step guide

**1) Invoking a scan**

As is, any `pull-request` event on this repo will invoke a scan. Simply create a PR with your desired dependency changes in `package.json`.

**2) Viewing results**

View verdicts in PR [comments](https://github.com/garnet-org/demos/pull/10#issuecomment-1489536753) and logs for the [workflow](https://github.com/garnet-org/demos/actions).

See [demo video](https://www.loom.com/share/d6662a575b41478fb4ddceef39ba1d57
).


**3) Customizing alerts (optional)**

- The [`rules.yml`](https://github.com/garnet-org/demos/blob/main/rules.yml) file contains a list of pre-defined `jq` expressions, which can be piped with lstn outputs to enforce policy.
- Setting the `rule-name` option to a name from the list (e.g. `block_priority medium`) will enforce that rule.
- You can also `ignore` certain behaviors, which means that CI won't be halted even if that rule condition is met.

Some examples:
 ```
  # Ignore medium priority detections 

  - name: ignore_priority_medium
    query: .[] | select(.verdicts[]?.priority == "medium")
    behavior: ignore
    
  # Halt CI if any outbound network connection is detected

  - name: block_network_connection
    query: .[] | .verdicts[]? | select(.message == "unexpected outbound connection destination")
  ```
### 🧰 Supported platforms

`lstn` currently supports JavaScript/TypeScript through the npm package manager. We're constantly expanding our ecosystem support, please [reach out](https://discord.gg/hvyUffjw) if you have any specific requests. 

### 📖 Documentation

Read about our detection [approach](https://docs.listen.dev/concepts/detection-approach), [issue coverage](https://docs.listen.dev/concepts/threat-coverage) and other concepts at [docs.listen.dev](https://docs.listen.dev/)

### 🔗 Connect with listen.dev tribe 

Hang out with us on [Discord](https://discord.gg/Tmavx64a), contribute to our projects on [GitHub](https://github.com/listendev), and contact our team directly at [support@garnet.ai](mailto:support@garnet.ai) 


![dolphin-3](https://github.com/garnet-org/demos/assets/3413596/265c7475-8b6c-408a-9a2b-228ec12e8232)

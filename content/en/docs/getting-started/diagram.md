---
title: "CI Diagram"
description: A diagram of how the CI components are connected.
weight: 1
---

## The View From Up High

At a high level, OCP CI uses [prow](https://github.com/kubernetes/test-infra/tree/master/prow) as the engine to drive the workloads. Events are received from `github` repositories and passed on to build farms, which in turn stand up `ephemeral` clusters for the `Openshift` tests to be run.

{{< inlineSVG file="/static/ocp-ci.svg" >}}

## The Core Repositories

The `repositories` highlighted below are the main locations of the configurations for the CI, the tools used with in the CI, and the tests that are run.

{{< inlineSVG file="/static/ocp-tools.svg" >}}

{{< row >}}
{{% column %}}

#### [openshift/release](https://github.com/openshift/release)

The `release` repository contains the ci configuration that the teams own under the `config` directory. Composable steps live in the `step-registry` and allow teams to build and re-use [workflows](http://localhost:1313/docs/architecture/step-registry/#workflow). All generated prowjob configs are contained in the `jobs` directory, but typically that won't be touched and is generated my `make` commands.

{{% /column %}}

{{% column %}}

#### [openshift/ci-tools](https://github.com/openshift/ci-tools)

The `ci-tools` repository is where most of the code for the different components used in our CI is located. The above highlights the `cmd` folder since that will contain the entrypoint for most of those tools. Some of note, are the `ci-operator` itself and `ci-operator-configresolver` which is used to host the UI for navigating the configurations.

{{% /column %}}

{{% column %}}

#### [openshift/origin](https://github.com/openshift/origin)

The `origin` repository is very straightforward for our purposes, it contains the `Openshift` end to end tests under the `extended` and `e2e/upgrade` directories, more information on writing and using those [tests](https://github.com/openshift/origin/tree/master/test/extended#openshift-extended-test-suite) can be found in the repo itself.

{{% /column %}}

{{< /row>}}

## The Cluster Flow

To round things off, the below diagram is taking a closer look at the cluster flow for the jobs. A list of current clusters can be found under the [Useful Links]({{< ref "/useful-links" >}})

The `prow` cluster will contain most of the prow services which are used to interact with github, filter requests, and route appropriate actions to take based on your configurations.

The tests jobs are then run on the build farm clusters, where your `workflow` will be executed. This typically, **but not always**, involves standing up ephemeral clusters on supported providers to run your tests.

{{< inlineSVG file="/static/ocp-clusters.svg" >}}

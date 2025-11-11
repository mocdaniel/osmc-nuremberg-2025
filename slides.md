---
theme: default
title: From Zero Observability to Zero-Code Observability with OpenTelemetry
info: |
  ## OSMC 2025
  September 18, 2025
class: text-center
transition: slide-left
mdc: true
---

# From Zero Observability to Zero-Code Observability with OpenTelemetry

<div class="mt-12 py-1">
  OSMC 2025<br/>
  September 18, 2025
</div>


<img src="/otel.png" alt="OTel logo" class="h-32 absolute top-80 right-20"/>
<img src="/osmc.svg" alt="OSMC logo" class="h-16 absolute top-5 right-95"/>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
layout: two-cols
class: text-xl
---

# About Me

- Senior Platform Advocate from Germany at **NETWAYS Web Services**
- Grafana Champion
- Especially interested in GitOps, Observability, IaC
- In my free time, I...
  - ...build small web apps
  - ...tinker with my homelab
  - ...learn about OpenTelemetry

::right::

<div class="h-full flex flex-col justify-center">
  <img src="/daniel-bodky.jpg" alt="Headshot of Daniel Bodky" class="w-64 rounded-full m-x-auto "/>
</div>

---
class: text-2xl
---

# About this Workshop

How to get started

<br />

Three simple steps:

1. Go to [osmc.nws.netways.de](https://osmc.nws.netways.de)
2. Enter the password: `osmc-otel-2025`
3. Click on **Start workshop**

---
class: text-2xl
---

# About Your Workshop Environments

What you find in there

<br />

- extensive dev tooling (`docker`, `git`, `npm`, etc.)
- tabs to view the applications you will be using (VS Code, Grafana, demo apps)
- these slides to follow along
- additional, interactive instructions for the hands-on parts

---
class: text-2xl
---

# Final preparations

Before we get started...

```sh
cd osmc-workshop
docker compose build
```

<style>
  code {
    @apply text-2xl;
  }

  .slidev-code {
    background-color: rgb(27, 27, 27);
    @apply mt-24;
  }
</style>

---
layout: section
---

# The Scenario

---

# The OSMC Explorer App

What we are dealing with

In order to instrument applications, we _need_ applications. Your environment contains a web application called **OSMC Explorer**.

It lets you:

- Browse talks chronologically
- View speaker bios
- Bookmark your favorite talks

You just built the microservices that make up the demo, written in various languages:

- `frontend` (NextJS/Typescript/NodeJS)
- `speaker-api` (Flask/Python)
- `talk-api` (Golang)

---

# Architecture Overview

How everything is connected

<img src="/osmc-overview.svg" alt="Architectural overview of the demo app"/>

---

# Initial Observability State

What can we see out of the box?

Initially, all microservices are **not instrumented** at all.

- No OTel SDKs
- No builtin `/metrics` endpoints
- Basic logging to see _something_ when interacting with the services

<br />

<br />

We are going to change that over the course of this workshop while exploring different ways of OpenTelemetry auto-instrumentation available.

---
layout: section
---

# Lab 1: Explore the Demo App

---
layout: section
---

# What is OpenTelemetry?

---
layout: two-cols
---

# OpenTelemetry (OTel)

An open observability standard

<br />
<br />

- Open-Source **observability framework**
- Provides **tools, APIs**, and **SDKs** to **collect, process**,
  and **export** telemetry data
- **Vendor-neutral** and **standardized**
- **Tracks** and **communicates** progress of the project's components on the [OpenTelemetry website](https://opentelemetry.io)

::right::

<div class="h-full flex flex-col justify-center">
  <img src="/otel.png" alt="OpenTelemetry logo" class="w-64 m-x-auto"/>
</div>

---
layout: two-cols
---

# OTel Signals

<br />

<br />

- **Metrics** for capturing metrics at runtime
- **Logs** for capturing timestamped text records
- **Traces** for capturing user interactions and events across service components
- **Profiles** for capturing code-level application performance and behavior at run-time (_still under development_)

::right::

<div class="m-l-4 h-full flex flex-col justify-center">

<img src="/otel-signals.svg" alt="Graphics of the four OTel signals" />

</div>

---

# Stability Overview Per Signal

How production-ready are the OTel SDKs?

| Signal | Stable | Beta | Development |
|----------|--------|-------|---|
| **Metrics** | <vscode-icons-file-type-go/> <vscode-icons-file-type-js-official/> <vscode-icons-file-type-python/>|||
| **Logs** | | <vscode-icons-file-type-go/> | <vscode-icons-file-type-js-official/> <vscode-icons-file-type-python/> |
| **Traces** | <vscode-icons-file-type-go/> <vscode-icons-file-type-js-official/> <vscode-icons-file-type-python/> | | |


<style>
  .slidev-icon {
    @apply w-10 h-10;
  }

  table {
    @apply w-full;
  }
</style>

---
class: text-xl
---

# OTel Instrumentation

SDKs and APIs

_For a system to be observable, it must be **instrumented**: that is, code from the system's components must emit signals, such as traces, metrics, and logs._ - [OTel Docs](https://opentelemetry.io/docs/concepts/instrumentation/)

- Two complementary strategies of instrumentation exist:
  - **Code-based**: deeper insights, rich telemetry from within the application itself
  - **Zero-code**: great for when you're just getting started or if you can't modify the application itself
- SDKs and APIs exist for various languages, in varying degrees of stability

<br />

➡️ **Today**, we are going to look at the **zero-code approach**

---
layout: section
---

# Lab 2: Instrumenting the First Application
# Installing CSML

As an open-source programming language, there are several ways you can get started with CSML, either by compiling the sources yourself, using Docker, or using [CSML Studio](https://studio.csml.dev), a free online development environment provided by [Clevy.io](https://clevy.io).

## No installation required: with CSML Studio

The easiest way to create CSML Chatbots is by using the free online development Studio available at [https://studio.csml.dev](https://studio.csml.dev/auth/register). No dependencies, no installation required: to get started, simply create a free account and head to the [Studio documentation](https://docs.csml.dev/studio/) to create your first bot!

CSML Studio includes everything you need to create, develop and maintain your chatbots. You get a code editor and test chatbox, built-in services to author functions and install custom or managed integrations, deploy to many different communication channels including Messenger, Workplace Chat, Microsoft Teams, Slack, Whatsapp... and monitor the usage with custom analytics.

**➡ Try CSML Studio now on** [**https://studio.csml.dev**](https://studio.csml.dev)**!**

## Install CSML Locally

To run CSML on your own machine, you will need a way to run the engine \(CSML comes with bindings for nodejs or directly in rust\), as well as a database. Optionally, you will also need a custom `App` runtime \([more about this on this page](custom-code-execution.md)\).

{% hint style="info" %}
[This blog post](https://blog.csml.dev/how-to-install-a-self-hosted-csml-engine-on-ubuntu-18-04/) explains how to run the CSML engine on your own server. It may be a useful starting point for a custom installation of CSML!
{% endhint %}

### With Docker

We provide an easy-to-use docker image to run CSML on your own computer. To get started, follow the instructions [on Docker Hub](https://hub.docker.com/r/clevy/csml-engine).

### Using a pre-built CSML Engine binary

We provide builds for macOS and linux \(arm64\) for every new version of the CSML engine. You can find them on the releases page of the [CSML Engine github repository](https://github.com/CSML-by-Clevy/csml-engine). For the latest available version, visit [https://github.com/CSML-by-Clevy/csml-engine/releases/latest](https://github.com/CSML-by-Clevy/csml-engine/releases/latest).

### Compile From Source

CSML is written in Rust. You can find the source code of the CSML interpreter and engine on [this github repository](https://github.com/CSML-by-Clevy/csml-engine), and compile it from source as per the instructions.

## 


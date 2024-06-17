---
layout: default
title: CICD-setup
nav_order: 1
description: "The official Google Summer of Code 2024 blog that will be used to keep research, progress diaries, and documentation up to date."
permalink: /CICD
has_children: false
---

# CI/CD Pipeline for BeagleBone AI 64 with Zephyr RTOS
This blog outlines a CI/CD pipeline for building and deploying Zephyr RTOS applications to a BeagleBone AI 64 board. The pipeline utilizes Docker for containerized builds and leverages SSH for secure deployment.

## Prerequisites:

Docker installed on your CI/CD runner
SSH keypair for BeagleBone access
BeagleBone AI 64 board
openbeagle account


## Pipeline Stages:

The pipeline consists of three main stages:

### Build:

- Creates a deployment directory.
- Initializes a Zephyr project workspace using west with the ai64-r5 branch from the glneo/zephyr repository.
- Updates the Zephyr project workspace.
- Exports the Zephyr build environment.
- Builds the Zephyr application (samples/hello_world) for the sk_am62/am6234/m4 target board.
- Copies the generated Zephyr ELF file (zephyr.elf) to the deployment directory.
- Artifacts:

- Saves the build artifact (deploy/zephyr.elf) upon successful build completion.
- Sets an expiration time of 28 days for the artifact.

### Deploy:

- Installs the openssh-client package within the Docker container for SSH functionality.
- Creates the SSH key directory and sets appropriate permissions.
- Adds the BeagleBone's host key to the known_hosts file.
- Securely copies the Zephyr ELF file (zephyr.elf) to the BeagleBone's /lib/firmware directory using SSH.
- Connects to the BeagleBone and executes commands via SSH to:
- Stop the remote processor.
- Load the Zephyr ELF file.
- Start the remote processor.

### Pipeline Variables:

1) SSH_PRIVATE_KEY: The private SSH key used to access the BeagleBone.
2) BEAGLEBONE_USER: The username for SSH access on the BeagleBone.
3) BEAGLEBONE_HOST: The hostname or IP address of the BeagleBone.
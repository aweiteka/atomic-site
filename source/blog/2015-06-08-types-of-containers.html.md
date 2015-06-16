---
title: Container Images Types
author: Aaron Weitekamp
date: 2015-06-08 17:56:10 UTC
tags: Docker, Containers, Applications
published: true
comments: true
---
As container use matures there are several different types of images emerging. I'm not referring to base vs. layered images. These are categories of images that describe how an image is used. The different categories of images are run by different types of users who have varying use cases in how they interact with and manage images. 

## Simple App Images

This is probably the most common type of image. This is the thing most people talk about when referencing containers.

**Example:** web-based or other applications

## System Services Images

These are daemon-ized images that provide services to a single host. These are common on the Atomic OS. Any agent or service that does not ship in the atomic tree needs to be added as a service container.

**Example:** rsyslog

## CLI Tools Images

These are images that replace traditional command-line tools. I've seen them run in two modes. For simple tools the tool can run once then exit. For more complex uses the user enters a container shell, executes one or more commands then exits. The shell model enables a "development environment in a container" type of user experience. Sometimes a suite of tools is available to achieve certain tasks.

**Example:** rhel-tools

## Service Component Images

These are images that developers integrate with. The most common example is the database service.

**Example:** database

### Technology Stack Component Images

components of another larger stack that are not meant for direct consumption

**Example:** openshift-pod

## Metadata Images

full Atomic apps. "Root" image.

**Example:** Wordpress-atomicapp

## Data-Only Images

data containers `--volumes-from`

**Example:**

##

Management and lifecycle of different types may 

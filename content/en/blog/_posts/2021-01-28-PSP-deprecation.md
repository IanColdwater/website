---
layout: blog
title: 'What’s Up with Pod Security Policies?'
date: 2021-02-04
slug: psp-update-2021
---

**Authors:** Tabitha Sable (SIG Security), Contributor Comms Team

_The following is a brief overview of upcoming changes to Kubernetes. Bookmark and [keep an eye on our release site](https://www.kubernetes.dev/resources/release/) to see the latest news and other important updates._
Starting with the release of Kubernetes 1.21, we will begin the [process of deprecation](/docs/reference/using-api/deprecation-policy/) for Pod Security Policies(PSP). This means that about one and a half years from now, PSPs will disappear from the newest Kubernetes release. In about two and a half years, PSPs will be fully removed from all supported Kubernetes releases. What this means for you depends on your situation; several good options are available. For more details, keep reading.

## What do Pod Security Policies do now?

> Pod Security Policy is an optional feature (Admission Controller) that allows an administrator to control security sensitive aspects of the Pod specifications in a Kubernetes cluster. The [PodSecurityPolicy](/docs/reference/kubernetes-api/policies-resources/pod-security-policy-v1beta1/) cluster-level object defines a set of conditions that a pod must meet in order to be accepted into the system. PSPs can also alter certain Pod specification fields, basically creating new defaults for those fields.

The [PodSecurityPolicy](/docs/concepts/policy/pod-security-policy/) documentation explains this in more detail.

## Why are Pod Security Policies being deprecated? 

SIG Auth summed it up beautifully in the description for their deep-dive session at KubeCon NA 2019: "Various limitations and structural problems have prevented the PSP API from (reaching General Availability)." In short, PSPs are cumbersome for both cluster operators and Kubernetes developers. Using PSPs is often unintuitive, and safely adding them to an existing cluster is impracticable.

PSP cannot be promoted to GA without solving these usability challenges, and the necessary changes are too radical to make while preserving backward compatibility. Having objects or features in Kubernetes that never reach GA (“permabeta”) is a problem for administrators and organizations, as a non-GA feature is not considered to be ready for widespread use. Because PSPs are trapped in "permabeta", they will be deprecated in Kubernetes 1.21. The current plan is to fully remove them in version 1.25.

For more backstory, check out the [deprecation PR](https://github.com/kubernetes/kubernetes/pull/97171) and the aforementioned KubeCon presentation:

{{< youtube "SFtHRmPuhEw?start=953" youtube-quote-sm >}}

## What will replace Pod Security Policies? 

Several options to replace Pod Security Policies are currently available or are in progress. We encourage you to assess the right tool for your needs--you have plenty of time. The general Kuberenetes way to add cluster features like this is via Admission Control Webhooks. [“A Guide to Kubernetes Admission Controllers”](/blog/2019/03/21/a-guide-to-kubernetes-admission-controllers/) gives a great overview of this capability.

Today, [a few tools in the CNCF ecosystem](https://landscape.cncf.io/card-mode?category=security-compliance&grouping=category), such as [OPA/Gatekeeper](https://github.com/open-policy-agent/gatekeeper/) and [Kyverno](https://github.com/kyverno/kyverno/), can enforce policies to achieve the same basic goals as PSP, and much, much more. Other active open-source projects, such as [K-Rail](https://github.com/cruise-automation/k-rail), are also working in this problem space. If you are not yet using Pod Security Policies and want to begin hardening your cluster with Pod admission control, you will likely find something that suits your unique needs.

The biggest advantage Pod Security Policies bring is the simplicity that comes from being built in to Kubernetes, but this promise remains out of reach because of PSP's usability problems. [SIG Auth](https://github.com/kubernetes/community/blob/master/sig-auth), [Kubernetes SIG Security](https://github.com/kubernetes/community/blob/master/sig-security), and other community members are working together to define and create a new built-in option. We plan to make it available as an Alpha feature in Kubernetes 1.22. If you're currently using PSPs and want to help shape the built-in replacement, please drop in to #sig-auth and/or #sig-security on Kubernetes slack and introduce yourself. We'd love to work together!

Securing your cluster by restricting Pod configuration requires understanding how the pods are intended to run and consideration of what types of behaviors or attack paths you seek to prevent. The [Pod Security Standards](/docs/concepts/security/pod-security-standards/) page in the Kubernetes documentation aims “to detail recommended Pod security profiles, decoupled from any specific instantiation.”  This page is a useful reference that answers common pod security questions and can help you consider what you may need from a native or external pod security enforcement tool.

## In Closing

Pod Security Policy was an early attempt to support users' needs to secure their clusters by controlling Pods. As Kubernetes has evolved since PSPs were created (almost 5 years ago!), they have become increasingly difficult to maintain and use. We know much more about running Kubernetes in production today, and we need to re-imagine PSPs to fix them. Thus, in Kuberntes 1.21, we are beginning the deprecation process for Pod Security Policy; we plan to fully remove the feature in 1.25.

The deprecation of PSP indicates the beginning of a long runway before it becomes unavailable. If you are currently using PSPs, this is a good time to begin evaluating the existing alternatives or contribute to the development of PSP's replacement.

Deprecation Release Notes: [https://relnotes.k8s.io/?kinds=deprecation](https://relnotes.k8s.io/?kinds=deprecation)

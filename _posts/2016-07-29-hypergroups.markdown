---
title: "Realizing Hypergroups as Association Schemes"
layout: post
date: 2016-07-29
img: hypergroups-front.jpeg
#tag:
#- Employment
#- Technology
#category: blog
#author: juntaeklee
description: Realizing Hypergroups as Association Schemes
---

## Table of Contents
1. [Project Brief](#project-brief)
3. [Reflection](#reflection)
4. [References](#references)

## Project Brief

Foundational to the undergraduate study of mathematics is understanding an algebraic structures called *groups*. A group consists of a set of elements and an operation that combines two elements and results in a third. The operation must also satisfy four properties (closure, associativity, identity, and invertibility). A simple example of a group is the set of all integers with the addition operation. As simple as they are, groups have been used to understand many physical phenomena because of an important theorem called *Cayley's Theorem*. Mathematician Arthur Cayley showed that every group is isomorphic to a group of permutations. In simple terms, groups can be used as a mathematical foundation to studying how any set of objects can be rearranged. For example, a Rubik's cube and its solutions can be fully described by using groups. Cayley's theorem shows us that we can realize groups as permutation groups.

In this project, I studied if there exists a parallel theorem to Cayley's theorem to the more abstract algebraic structure called hypergroups. Hypergroups adhere to all of the properties of groups but the operation combines two elements and results in a *subset* of elements instead of a single third element. The parallel to Cayley's theorem with hypergroups replaces permutation groups with *association schemes*. Unfortunately, we found that there is no strict isomorphic equivalence between hypergroups and association schemes, so instead I explored which hypergroups can and cannot be realized as association schemes.

In particular, I studied all 37 known nonsymmetric hypergroups of rank 4 to prove if they can or cannot be realized as association schemes. I proved that 11 hypergroups can be realized as finite association schemes and 20 could not. The full document describing the results of the study can be found below:

<iframe src="/assets/doc/HypergroupsAssociationSchemes_leejuntaek.pdf" width="100%" height="400px"></iframe>

## Reflection

 - While the project was focused on hypergroups of rank 4, I found it immensely helpful to carefully study the less complicated hypergroups of rank 3 to build intuition. Research had already proven the association schemes hypergroups of rank 3 can be realized as, but by exploring these hypergroups I was able to generate additional lemmas that proved helpful for those of rank 4. Intuition in pure mathematics can be detrimental to rigorous proofs but it helps build a mental framework around any complicated system.

 - Much of the research was focused on proving that some hypergroups can or cannot be realized as association schemes, without necessarily knowing the actual association schemes they can be realized as, but using computational methods to actually generate certain association schemes made results more concrete. The generated association schemes did not serve to solidify proofs but they helped build intuition. Using computational methods to find inspiration was tremendously helpful when hitting mental blocks towards proofs.

## References

https://i.ytimg.com/vi/5cSb8kwyrMM/maxresdefault.jpg
---
title: "[Technique] who_came_first"
date: 2021-04-19T14:05:07+07:00
draft: true
---

# Who came first 
19/04/2021

This post contains 3 parts:
1. Introduction
2. Explanation
3. Some problems

## I. Introduction
The technique is quite simple but I feel it attractive and interesting in some ways. I think I across this technique a few times before but I haven't noticed it. So today I decide to write that down to satisfy myself.

## II. Explanation
Assuming you want to construct a fixed-size subset of an array of `{1, 2, ..., n}` in a way that sum of all elements is equal to $S$.
This technique helps to construct it in a effective time complexity way.
You have an array $A = [1, 2, ..., n]$ 
You want to construct an subset of k element which have sum equal to $S$.
We can see the result subset will be in format subset = {a1, a2, ..., ak}
Without loss of generality, let's sort subset to become a1 < a2 < ... < ak.

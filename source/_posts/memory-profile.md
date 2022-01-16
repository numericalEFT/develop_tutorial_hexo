---
title: Memory Profile
date: 2022-01-15 20:35:00
tags:
---

Memory allocation is expansive. It is important to find the unecessary memory allocation in the code. You may profile the memory usage of your code with the following steps. 

- Run command:
```sh
julia --track-allocation=user script.jl
```
- files *.mem will be generated for each script
- To clean up *.mem file, run command:
```sh
find . -name "*.mem" -type f -delete
```

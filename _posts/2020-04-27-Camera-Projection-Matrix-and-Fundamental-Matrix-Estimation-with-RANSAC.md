---
layout: post
title: "Zhimin Sun, Camera Projection Matrix and Fundamental Matrix Estimation with RANSAC"
date: 2020-04-27
---

Hi, this blog is to record the project related to Camera projection matrix and fundamental matrix estimation.

Part 1: Camera projection matrix

Def.:For a pinhole camera model, the camera matrix is ${P} \in {R}$ <sup> $3 \times 4$ </sup> is a projective mapping from world(3D) to pixel(2D) coordinates defined up to a scale.

The camera matrix can also be decomposed into intrinsic parameters K and entrinsic parameter $R^T [ I | -t]$.

P = K $R^T [I | -t]$.

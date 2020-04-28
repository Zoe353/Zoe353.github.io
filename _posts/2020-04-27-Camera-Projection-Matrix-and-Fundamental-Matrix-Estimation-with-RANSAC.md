---
layout: post
title: "Zhimin Sun, Camera Projection Matrix and Fundamental Matrix Estimation with RANSAC"
date: 2020-04-27
---
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>


Hi, this blog is to record the project related to Camera projection matrix and fundamental matrix estimation.

Part 1: Camera projection matrix

Def.:For a pinhole camera model, the camera matrix is ${P} \in {R}$ <sup> $3 \times 4$ </sup> is a projective mapping from world(3D) to pixel(2D) coordinates defined up to a scale.

The camera matrix can also be decomposed into intrinsic parameters K and entrinsic parameter R <sup>T</sup>[I|-t].

P = K R <sup>T</sup>[I|-t]. 

Let's Look more carefully into what each individuals parts of the decomposed matrix mean.
<li>The homogenous vector coordinates(x<sub> w </sub>, y<sub> w </sub>, z<sub> w </sub>, 1) of X<sub>w</sub> indicate the position of a point in 3D space in the world coordinate system.</li> 
<li>The matrix [I|-t] represents a translation</li>
<li>matrix R<sup> T</sup> represents a rotation.</li>

Insert the matrix represention of the projection matrix.

An intuitive way to understand this is to think about how aligning the axes of the world coordinate system to the ones of the camera coordinate system can be done with a rotation and a translation.

A camera projection matrix maps points from 3D into 2D. We can use this to estimate its parameters. Assume that we have N known 2D-3D correspondences for a set of points, that is, for points with index i=1...N we have both access to the respective 3D corrdinates x<sub>w</sub><sup>i</sup> and 2D coordinates x<sup>i</sup>.$$x_i^w$$

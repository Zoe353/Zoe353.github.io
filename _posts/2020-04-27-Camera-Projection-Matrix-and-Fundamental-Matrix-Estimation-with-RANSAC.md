---
layout: post
title: "Zhimin Sun, Camera Projection Matrix and Fundamental Matrix Estimation with RANSAC"
markdown: kramdown
date: 2020-04-27
---

Hi, this blog is to record the project related to Camera projection matrix and fundamental matrix estimation.

Part 1: Camera projection matrix

Def.:For a pinhole camera model, the camera matrix is $P \in R^{3 \times 4}$ is a projective mapping from world(3D) to pixel(2D) coordinates defined up to a scale.

The camera matrix can also be decomposed into intrinsic parameters K and entrinsic parameter $$R^T \[ I|-t \] $$.

$$ P = K R^T \[ I \| -t \] $$

Look more carefully into what each individuals parts of the decomposed matrix mean.
<li>The homogenous vector coordinates $(x_w,y_w,z_w,1)$ of $X_w$ indicate the position of a point in 3D space in the world coordinate system.</li> 
<li>The matrix [I|-t] represents a translation</li>
<li>matrix $R^T$ represents a rotation.</li>

Insert the matrix represention of the projection matrix.

An intuitive way to understand this is to think about how aligning the axes of the world coordinate system to the ones of the camera coordinate system can be done with a rotation and a translation.

A camera projection matrix maps points from 3D into 2D. We can use this to estimate its parameters. Assume that we have N known 2D-3D correspondences for a set of points, that is, for points with index i=1...N we have both access to the respective 3D corrdinates $x_w^i$ and 2D coordinates $x^i$. Let $P\hat {^}$ be an estimation for the camera projection matrix. We can determine how accurate the estimation is by measuring the reprojection error between the 3D points projected into 2D and the known 2D points, both in homogeneous coordinates.

Optimizing the reprojection loss using Levenberg-Marquardt requires a good initial estimate for P. This can be done by having good initial estimates for K and R<sup>T</sup> and t which you can multiply to then generate your estimated K.

Part 2: Fundamental matrix

Def: Fundamental matrix($$F$$) maps corresponding 2D points from two images of the same scene.

To estimate $$F$$ we can minimize the line-to-point distance between the point $$x_0$$ and the line $$Fx_1$$, for all matching points $$(x_0, x_1)$$. This makes estimating the fundamental matrix a least squares problem.








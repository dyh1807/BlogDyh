---
title: math-formula
date: 2023-05-28 15:32:56
tags:
- Math
- Linear Algebra
- Calculus
categories:
- Math
- Formula
---

> Regular Formula

## Matrix/Vector Derivation

> [https://en.wikipedia.org/wiki/Matrix_calculus](https://en.wikipedia.org/wiki/Matrix_calculus)

> Use denominator layout by default

### vector by vector
suppose x is a vector(column vector by default), using denominator layout:

- $$ \frac{\partial Ax}{\partial x} = A^T$$
- $$ \frac{\partial x^T A}{\partial x} = A$$
- $$ \frac{\partial u^T A v}{\partial x} = \frac{\partial u}{\partial x}A v + \frac{\partial v}{\partial x}A^T u$$
- $$ \frac{\partial x^T A x}{\partial x} = (A + A ^ T) x$$
- $$ \frac{\partial u ^ T v}{\partial x} = \frac{\partial u}{\partial x} v + \frac{\partial v}{\partial x} u$$

### chain rule

- $$ \frac{\partial f(g(u))}{\partial x} = \frac{\partial u(x)}{\partial x}\frac{\partial g(u)}{u}\frac{\partial f(g)}{\partial g}$$
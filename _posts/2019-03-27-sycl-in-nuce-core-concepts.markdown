---
layout: post
title:  "SYCL *in nuce* -- Introduction and Core Concepts"
date:   2019-03-27 13:30:00 +0100
categories: sycl tutorial
---

# Introducing SYCL

The [SYCL standard][syclspec] was initially released in 2014 by the Khronos
consortium and builds upon OpenCL's concepts for hardware abstraction and
cross-platform portability while offering a modern single-source C++ API at
the same time.

TODO

## Hello World

A commonly used function from the *Basic Linear Algebra Subroutines* (BLAS)
library is SAXPY which stands for *single-precision a · X + Y*, where a is a
scalar value and X and Y are vectors. If you wanted to implement this in
standard C++ you would probably use the `std::transform` function:

{% highlight c++ %}
void saxpy(float a, const std::vector<float>& x_vec, std::vector<float>& y_vec)
{
    std::transform(begin(x_vec), end(x_vec), // first input iterator
                   begin(y_vec),             // second input iterator
                   begin(y_vec),             // output iterator
                   [a](const float& x, const float& y)
                   {
                       // apply the following line to all elements in X and Y
                       // and write result to output vector
                       return a * x + y;
                   });
}
{% endhighlight %}

This is roughly equivalent to something like this:

{% highlight c++ %}
void saxpy(float a, const std::vector<float>& x_vec, std::vector<float>& y_vec)
{
    for(auto i = 0; i < x_vec.size(); ++i)
    {
        y_vec[i] += a * x_vec[i];
    }
}
{% endhighlight %}

# Core Concepts

## Parallelization



## Task Graphing


## Execution Model

## Memory Model

## System View

[syclspec]: https://www.khronos.org/registry/SYCL/
[computecpp]: https://developer.codeplay.com/products/computecpp/ce/home/
[triSYCL]: https://github.com/triSYCL/triSYCL
[Intel]: https://github.com/intel/llvm/tree/sycl


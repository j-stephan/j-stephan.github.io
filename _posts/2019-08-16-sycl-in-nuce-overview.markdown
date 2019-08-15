---
layout: post
title:  "SYCL *in nuce* -- Overview"
date:   2019-08-16 00:00:00 +0100
categories: sycl tutorial
---

This chapter will provide a high-level view of SYCL and the problems it is meant
to solve. After that we will look at the steps required for the offloading of
a basic SAXPY operation. The chapter ends by covering the structure of the
SYCL API.

# Overview

## Origins

## What it takes to offload a SAXPY operation

### Step 1 - Device selection and queue creation

### Step 2 - Device memory allocation

### Step 3 - Kernel execution

### Step 4 - Synchronization

### Summary

# API concepts

## Coding conventions

At the moment SYCL is only available for the C++ programming language. There are
no bindings for C, Fortran or any other language as far as I know.

All of SYCL's functionality is defined inside the `sycl.hpp` header which is
supplied along your vendor's SYCL compiler. We will cover the installation of
this environment in the next chapter.

SYCL's classes, constants, types and functions reside inside the `cl::sycl`
namespace. Vendor-specific extensions are defined inside
`cl::sycl::<vendor_name>`.

Object creation usually relies on a lot of `enum class`es in conjunction with
`template` parameters. In general, object creation follows this pattern:

{% highlight c++ %}
auto sycl_object = cl::sycl::sycl_class<int,             // a data type
                                        1,               // dimensionality of the object
                                        cl::sycl::config // a SYCL type or enum class value
                                        >{
                        cl::sycl::range<1> // usually the same dimensionality as above
                        {42}               // the object's size
                    };
{% endhighlight %}

## Reference semantics

One important difference to ordinary C++ are SYCL's reference semantics,
especially when copying SYCL objects on the host side. The specification states
in section 4.3.2:

> Each of the following SYCL runtime classes: `device`, `context`, `queue`,
> `program`, `kernel`, `event`, `buffer`, `image`, `sampler`, `accessor` and
> `stream` must obey the following statements, where `T` is the runtime class
> type: [...]
> Any instance of `T` that is constructed as a copy of another instance, via
> either the copy constructor or copy assignment operator, must behave as-if it
> were the original instance and as-if any action performed on it were also
> performed on the original instance and if said instance is not a host object
> must represent and continue to represent the same underlying OpenCL objects
> as the original instance where applicable.

Note that this also refers to memory buffers and images! This means that a
copy performed by assignment only creates a reference to the very same
memory, it does not allocate new memory or copies any of the existing device
memory. This is different from the assignment operations we know from standard
C++ where an assignment would create (for example) a new `vector` and then
perform a copy of the old `vector`'s contents.

## Exception handling

SYCL's method of error handling are exceptions. While these are similar to the
exceptions known from the C++ STL they do not inherit from any of the STL's
predefined classes. Instead, SYCL defines a base `exception` class from which
multiple other exception types inherit.

By default SYCL will only (visibly) throw exceptions that occur on the host
side, i.e. during program control flow. Exceptions that are raised on the device
side (inside the kernel) will be thrown asynchronously with regard to the host
program. The programmer needs to set up an asynchronous exception handling
mechanism if he wants to catch these errors (we will cover this in a later
chapter).

## Profiling

Sooner or later you will want to profile your device kernels. For basic
timing-based profiling you can use SYCL's built-in functionality: just pass
the `enable_profiling` property to the `queue` during the latter's construction
and you can query the `event`s returned by the `queue` for profiling
information. We will cover this with more detail in one of the following
chapters.


[syclspec]: https://www.khronos.org/registry/SYCL/


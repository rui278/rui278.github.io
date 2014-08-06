---
Title:	OpenCL is cool (plus an unexpected explanation of k-means clustering)
Layout:	Post
Comments:	True
---

OpenCL is cool, and that’s a not a useful statement…

##Ok. Backstory.

I used GPGPU last semester for an assignment, in which we had to accelerate one algorithm using the CUDA platform. My group’s algorithm was [Kmeans clustering](https://en.wikipedia.org/wiki/Kmeans). For some context, this algorithm is pretty simple. All it does is to pass through a number of data points and assign them to a cluster, given a set number of clusters and their initial [centroids](https://en.wikipedia.org/wiki/Kmeans). 
The implementation of the algorithm is fairly simple. You basically break it down into three logical components and two logical for loops. The logical components are are as follows: 

<b>For Loop 1 - Classification </b>: basically the gist of the algorithm, you run through all your data points and classify them to a specific cluster.

<b>For Loop 1 - Accumulation: </b>in which you do the summation of the coordinates (individually) of all the data points classified to a cluster and count the number of data points attributed to each cluster. (This will make sense in the next component).

<b>For Loop 2</b> - Recalculation: using the summation of all the coordinates data points in the cluster and the number of points you create a new centroid by dividing those two metrics, so that the new centroid corresponds to the “center of mass” of all the points that are attributed to him (implies that the selection of original centroids is important to the output, and assuming all data points have the same weight in the recalculation).

And then this repeats itself until either you have a set number of iterations or recalculated centroids don’t change at all.

(I don’t know why i had to explain all of the algorithm when i liked it… Too late i guess…)

##Actual blog post content that’s not just me sidetracking into other topics...
All of this, just to say, I learned some CUDA and used a GPU ([Nvidia K40](http://www.nvidia.com/object/tesla-servers.html)).

Now, my thesis is also about GPGPU, so i started to learn about OpenCL. I figured it’d be about the same as CUDA. And it it, in its core they both use the same structure. A host talks to a device (or multiple) and send jobs(kernels) to it. Easy peasy. And that’s basically what I did in that assignment. Copied data to GPU, executed kmeans kernel, copied data back to CPU, got time, calculated speedup. And that was it.

So for me, that was GPGPU.

But then i started to read about OpenCL, and it’s got loads of cool features. 

First the job submitting structures: the queues. Basically you submit a job by queueing it in a per device queue. That queue can be managed in order, or out of order, which is pretty cool when interacting with other devices, and that same interaction is also pretty awesome. You can synchronize various kernel launches by using events.

This brings out loads of possibilities to make the execution really efficient and adapt really well in runtime to any kind of device configuration. For example:

kernel A on device 0 needs to wait for kernel B’s finish in device 1. Then on device 0’s queue, after A, comes kernel E that does not depend on any other kernel. 

With synchronous management of the queues we can for example almost ignore the fact that we have a slower device 1. For example, in a system, kernel B finishes up before kernel A is up for execution in one system. But in another, system, the GPU is worse, and so kernel A cannot be launched, but because it’s an out of order queue we can just send in kernel E. 

Now, this is awesome! And a really great tool to make a portable application that can still be somewhat consistent in terms of performance in various different systems!

So i conclude, OpenCL is cool!

First post. YAY.

Also, great tool to learn openCL: [AMD's Programing in OpenCL videos](http://developer.amd.com/tools-and-sdks/opencl-zone/opencl-resources/programming-in-opencl/)

-Rui

(P.S. I really only know CUDA in a very superficial way. I have no notion of it’s capabilities when it comes to managing jobs. And I’m absolutely not saying OpenCL is great in detriment to CUDA, nor making any assumption of it’s capabilities, or comparing them. All I’m saying is, i was expecting a level of finesse from GPGPU, and the actual level i got, is surprisingly cool! So, in reality, what I’m saying is that GPGPU programming is more awesome than i thought it’d be, and the vehicle for that realisation is OpenCL!).

{% include shamelessPlug.html %}

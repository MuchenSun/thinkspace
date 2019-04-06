---
layout: post
title: "GSoC 2019 MBDyn Entry Test"
comments: true
description: "GSoC 2019 MBDyn Entry Test"
keywords: "GScoC, MBDyn"
---
## Introduction
In this post, I will describe how I finish the entry tests of MBDyn for GSoC 2019.

## Step 1: First Contact
### 1.1 Compilation of MBDyn
* Platform: Ubuntu 16.04
* Download MBDyn from [https://www.mbdyn.org/?Software_Download](https://www.mbdyn.org/?Software_Download), here I download the 1.7.3 version. After that, extract the file and enter the directory.
* Enter the "mbdyn-1.7.3" directory in terminal, execute the following commands:
~~~
./configure
make
sudo make install (In my case, if I don't add "sudo" before "make install", it will give me a "
recipe for target 'uninstall-recursive' failed" error)
~~~
* Then type "mbdyn" in terminal to test. In my case, I had a "command not found" error. According to the [maillist](http://mail.mbdyn.org/pipermail/mbdyn-users/2019-March/002231.html), I added "export PATH=${PATH}:/usr/local/mbdyn/bin" in my .bashrc file and the problem is solved.

### 1.2 Test case
I modified the the "free rigid body" example in the [MBDyn tutorial](https://www.mbdyn.org/userfiles/documents/tutorials.pdf). I setup rigid body and gave them different initial velocity. 
Here is the source code:
~~~
begin: data;
	problem: initial value;
end: data;

begin: initial value;
	initial time: 0.;
	final time: 1.;
	time step: 1.e-3;

	max iterations: 10;
	tolerance: 1.e-6;
end: initial value;

begin: control data;
	structural nodes: 2;
	rigid bodies: 2;
	forces: 2;
end: control data;

begin: nodes;
	# node number 1, dynamic, placed in [0,0,0], rotation matrix is an identical matrix, initial velocity is [0.,1.,0.], no angular velocity;
	structural: 1, dynamic, null, eye, 0.,1.,0., null; 

	# node number 2, with a different initial velocity;
	structural: 2, dynamic, null, eye, 0.,3.,0., null;
end: nodes;

begin: elements;
	# body number 1, attached to node number 1, mass is 1., initialized at the origin of the reference frame, inertia matrix is an identity matrix;
	body: 1, 1, 1., null, eye;

	# body number 2, with a different mass
	body: 2, 2, 10., null, eye;
	
	# force number 1, absolute force, attached to node number 1, directed as axis 3 in reference frame, has no arm with the node, amplitude is constant -9.81(gravity);
	force: 1, absolute, 1, 0.,0.,1., null, const, -9.81;

	# force number 2, the rest are same with number 1
	force: 2, absolute, 2, 0.,0.,1., null, const, -9.81;
end: elements;
~~~

The plot result is:
![plot result 1](https://raw.githubusercontent.com/MuchenSun/blog/master/img/mbdyn_entry_test_1.png)
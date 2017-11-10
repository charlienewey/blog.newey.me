---
layout: post
title: Parallel I/O and JASMIN
date: 2014-07-16 22:23:23.000000000 +01:00
---

So further to [my last post](http://blog.newey.me/parallel-processing-python/),
I've been putting some parallel Python code to use... at work! I currently work
at [CEDA](http://www.ceda.ac.uk) in Oxfordshire, using a number of the
[JASMIN](http://www.jasmin.ac.uk/) virtual machines to analyse and extract
information from scientific datasets. This involves processing tens of
thousands of files - ranging from a few kilobytes to 7-8 gigabytes in size. To
make this task feasible, I need access to some considerable hardware - which
CEDA has in the form of JASMIN.

<!-- more -->

The JASMIN processing nodes are high-performance, multi-core virtual machines
backed by (fast) parallel [Panasas](http://www.panasas.com/) storage, and they
are part of CEDA's data-intensive processing and archival infrastructure (the
same infrastructure that provides the
[LOTUS](http://proj.badc.rl.ac.uk/cedaservices/wiki/JASMIN/LOTUS) cluster).
JASMIN analysis environments are quickly gaining popularity amongst the
atmospheric and earth observation scientific communities for processing
extremely large datasets.

Essentially, the JASMIN virtual machines are deployed with a pre-cooked image
of a Linux system (currently RHEL), and these VMs are then set up with a number
of pre-configured packages that are useful for data analysis tasks - namely
Python, SciPy, iPython, R and an impressive array of other tools.

So, while [my last post](http://blog.newey.me/parallel-processing-python/)
talked about performing CPU-bound tasks in parallel using Python - this post
will cover what happens when using parallel Python code for more IO-bound
tasks.

For this example, I'm using Python to generate MD5 hashes of several large
files - approximately 5GB each. I am using 3 methods of processing the files
for comparison - processing the files in serial (one after the other), using
Python's `threading` module, and  using Python's `multiprocessing` module. In
my last post, `multiprocessing` was the best option for running tasks in
parallel, but I wonder if the outcome will be the same for more I/O intensive
tasks... The scripts I used to perform this experiment are below, along with
the [results](#results).

Serial/standard:
```language-python
import hashlib
import os

def calculate_md5(fname):
    with open(fname, 'r') as f:
        rdsize = (os.fstatvfs(f.fileno()).f_bsize)*64

        h = hashlib.md5()
        buf = f.read(rdsize)
        while len(buf) > 0:
            h.update(buf)
            buf = f.read(rdsize)

        fn = os.path.basename(fname)
        print("%s: %s" % (fn, h.hexdigest()))

files = ["APEX_HYME2_120910_a03r_Part_0_R.img",
		 "APEX_HYME2_120910_a03r_Part_1_R.img"]

for f in files:
	# Hash the file!
    calculate_md5(f)
```


Threading:
```language-python
import hashlib
import os
import threading
import time


def calculate_md5(fname):
    with open(fname, 'r') as f:
        rdsize = (os.fstatvfs(f.fileno()).f_bsize)*64

        h = hashlib.md5()
        buf = f.read(rdsize)
        while len(buf) > 0:
            h.update(buf)
            buf = f.read(rdsize)

        fn = os.path.basename(fname)
        print("%s: %s" % (fn, h.hexdigest()))

files = ["APEX_HYME2_120910_a03r_Part_0_R.img",
		 "APEX_HYME2_120910_a03r_Part_1_R.img"]

for f in files:
    # Hash the file!
    t = threading.Thread(target=calculate_md5, args=(f,))
    t.start()

while threading.active_count() > 1:
    time.sleep(1)
```

Multiprocessing:
```language-python
import hashlib
import multiprocessing
import os
import time

def calculate_md5(fname):
    with open(fname, 'r') as f:
        rdsize = (os.fstatvfs(f.fileno()).f_bsize)*64

        h = hashlib.md5()
        buf = f.read(rdsize)
        while len(buf) > 0:
            h.update(buf)
            buf = f.read(rdsize)

        fn = os.path.basename(fname)
        print("%s: %s" % (fn, h.hexdigest()))

files = ["APEX_HYME2_120910_a03r_Part_0_R.img",
		 "APEX_HYME2_120910_a03r_Part_1_R.img"]

for f in files:
    # Hash the file!
    m = multiprocessing.Process(target=calculate_md5, args=(f,))
    m.start()

while len(multiprocessing.active_children()) > 0:
    time.sleep(1)
```

<a name="results"></a>

#### Results

```language-bash
ccnewey@jasmin-sci2 ~/mpp $ time python serial.py
real	0m17.661s
user	0m15.351s
sys	0m2.311s

ccnewey@jasmin-sci2 ~/mpp $ time python threaded.py
real	0m18.136s
user	0m17.849s
sys	0m7.514s

ccnewey@jasmin-sci2 ~/mpp $ time python multiprocess.py
real	0m9.183s
user	0m24.104s
sys	0m3.344s
```

As is evident from the looking at the **"real"** row in the `typescript` above,
the code that uses the Python `multiprocessing` module is extremely fast -
running in roughly half the time of the serial and threaded code! The threaded
code again, appears to be slowest of all - although, the difference is not as
pronounced as in my last post, because the code in this post is mostly I/O
bound - that is, most of the time is spent waiting for I/O to stop blocking
execution.

I was initially rather surprised by this result - it seemed strange to me that
using parallel processing would improve the execution time of what was
essentially an I/O-bound task. It occurred to me after a while that JASMIN's
Panasas storage supports parallel access - an application can perform read
operations on multiple files simultaneously, thus improving the speed at which
tasks like calculating MD5 hashes can run.

#### Conclusions

The high-speed parallel storage that backs the JASMIN cluster is excellent for
improving the parallelisation of data processing scripts, thus enabling
atmospheric and climate scientists to produce larger volumes of data in a
shorter period of time.

Python's `multiprocessing` module is an excellent addition to any Python
programmer's toolkit, and it should be used in preference of the `threading`
module.

#### Honourable mention

Python 3 has integrated a [new asynchronous I/O
module](https://docs.python.org/3/library/asyncio.html) - `asyncio`, which is
supposed to remove a lot of the existing issues with blocking I/O in Python 2.
I haven't used it myself yet, but there are some [interesting
videos](http://www.youtube.com/watch?v=1coLC-MUCJc) on the topic.

### Resources
* [JASMIN](http://www.jasmin.ac.uk/)
* [National Environment Research Council](http://www.nerc.ac.uk/)
* [Centre for Environmental Data Archival](http://www.ceda.ac.uk/)
* [British Atmospheric Data Centre](http://www.badc.ac.uk/)
* [Python `multiprocessing` module](https://docs.python.org/2/library/multiprocessing.html)
* [Python `asyncio` module](https://docs.python.org/3/library/asyncio.html)

The requirements for ShareLaTeX vary on the level of activity. The largest use of resources is the compiling of tex. As a rule of thumb we recommend a base of 2GB of memory and 2 cpu cores for the docker image. Then 1 core and 1GB of memory for every 10 concurrent users. If users are compiling heavily then this might need to be increased to every 5 users. i.e.

10 people working at same time = 3 cpu cores + 3GB memory
50 people working at same time = 6 cpu cores + 6GB memory

These resources do not include the dependencies mongodb and redis. Which would likely need 1GB of memory and 1 Cores together for a small instance.

It is possible to run ShareLaTeX over multiple servers, see the [load balancing documentation for more information](https://github.com/sharelatex/sharelatex/wiki/Load-balancing-ShareLaTeX-and-Haproxy).
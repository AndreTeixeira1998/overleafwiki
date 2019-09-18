## Minimal install

A base requirement of 2 cores and 3GB memory is required for basic operations with around 5 concurrent users. 

## Larger installs
When provisioning hardware to run Overleaf the main factor is how many concurrent users will be running compiles. Depending on how large the documents are and how often users compile as a rule of thumb 1 cpu core and 1gb of memory should be added to the minimal install for every 5 concurrent users. So if 30 people are working on Overleaf at the same time, 8GB and 7 Cores (5 cores + 5GB + base of 2 cores & 3GB) would provide generious resources for your users.

## CPU speed
LaTeX is a single threaded program, meaning it can only utilize one cpu core at a time. The CPU is also the main limitation when compiling a document. Therefore the faster the single core performance of your CPU the faster you will be able to compile a document. More cores will only help if you are trying to compile more documents than you have free cpu cores.
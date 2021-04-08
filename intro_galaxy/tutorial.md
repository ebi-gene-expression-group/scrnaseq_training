# Introduction to Galaxy

In this part we will learn what is Galaxy and what are the main mechanistics involved in using it. 
We will start with [these slides](https://training.galaxyproject.org/training-material/topics/introduction/slides/introduction.html#1).

## Using Galaxy

We will start our practical demonstration by re-using this [introductory tutorial](https://training.galaxyproject.org/training-material/topics/introduction/tutorials/galaxy-intro-short/tutorial.html).

If you are interested in further improving your Galaxy user skills, there are plenty more introductory tutorials [here](https://training.galaxyproject.org/training-material/topics/introduction/).

# Where can I use this later on?

All of the tools and workflows used during the course (introductory material and single cell specific material) are available and can be used from https://humancellatlas.usegalaxy.eu/. In some cases, for instance for Interactive Tools, you might want to access these tools with the same user credentials at https://usegalaxy.eu/ (the Human Cell Atlas instance is a facade to usegalaxy.eu which exposes more prominently our single cell tools, but users and data remain the same). For additional background, you can read our [paper on Nature Methods](https://www.nature.com/articles/s41592-021-01102-w).

# Can I use them on a different Galaxy instance?

All of the tools that we are using during this course are publicly available at the [central Galaxy Toolshed](https://toolshed.g2.bx.psu.edu/) (most of them from our user [ebi-gxa](https://toolshed.g2.bx.psu.edu/view/ebi-gxa)), which means that they can be installed on any Galaxy setup.

- Public Galaxy instances: [usegalaxy.eu](https://usegalaxy.eu/), [usegalaxy.org](https://usegalaxy.org/), [usegalaxy.au](https://usegalaxy.org.au/) and [humancellatlas.usegalaxy.eu](https://humancellatlas.usegalaxy.eu/). There are more listed instanced [here](https://galaxyproject.org/use/). If the tools that you need are not there, talk to the administrators.
- Institutional instance: If your institute hosts a Galaxy instance, you can ask the administrator to install them for you from the toolshed.
- On the cloud: If you have cloud credits with a cloud provider, you can spin up your own Galaxy setup for the time that you need it through either [cloudman](https://galaxyproject.org/cloudman/) or the Galaxy-Kubernetes setup.
- Locally on your machine: if you have a linux machine and you feel comfortable with the command line, you can checkout the Galaxy code and execute `./run.sh`. This will be of course limited in terms of computational resources, but for small datasets it should be fine.

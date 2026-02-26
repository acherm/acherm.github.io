---
layout: post
title:  "Docker New Inactive Image Policy and Reproducible Science"
date:   2020-09-01 011:54:29 +0200
tags: [reproducibility, docker, experiments, science]
---

Docker is a popular technology for delivering software in so-called containers. It is mainly used for deploying applications on the cloud, but is also widely considered in the scientific community. Docker Inc. has introduced a new inactive image retention policy: in short, images that have not been pulled or pushed in 6 months are considered as inactive and will be removed. In this blog post, I'm briefly discussing what could be the possible impacts on science, based on my experience and some concrete cases. I have no silver bullet but something is clear: scientists should react now and discuss/find alternate, sustainable solutions. 



## Docker new policy

Docker [announcement](https://www.docker.com/blog/scaling-dockers-business-to-serve-millions-more-developers-storage/):  

>  To help Docker economically scale its infrastructure to support free services for our growing base of users, several updates were announced. First, a new inactive image retention policy was introduced that will automatically delete images hosted in free accounts that have not been used in 6 months. In addition, Docker will also be providing tooling, in the form of a UI and APIs, that will allow users to more easily manage their images. Together, these changes will allow developers to more easily clean up their inactive images and also ensure Docker can economically scale its infrastructure. With this new policy, starting on **November 1**, images stored in free Docker Hub repositories that have not had their manifest pushed or pulled in the last 6 months will be removed.



This new policy is frankly understandable (e.g., from an economic point of view). But incidentally it can be a big threat to reproducibility in science. Many scientific works indeed rely on Docker. For instance, in software engineering, I'm seeing more and more artefacts linked to a paper (nice!) and such artefacts are sometimes Docker images. There are good reasons: 

* it's more convenient to have a ready-to-use Docker image: building Docker images can be quite long, require lots of resources in bandwidth, CPU or memory;
* you can well provide the Docker files and assume everything will work "in theory". In practice, many problems can occur: the build of some tools/libraries fails, some related artefacts are missing, the versions you assume are no longer working with other pieces, etc. 
* Even if everything builds in the first place, it's quite hard to guarantee that the produced Docker image is exactly the one used for your experiments. For example, some versions of a library may have changed: they do work from a build point of view, but the results are then completely different.  

So I would argue that providing the Docker file is a good practice but is not enough. One needs to retain the concrete Docker images. Some people may argue that Docker is broken by design and that for real reproducibility one needs other tools like Nix, Singularity, etc. I will get back to this after. 

## Impacts on my research 

But first, let me report the impacts of Docker new policy on my research work. If my colleagues and I do nothing, what could be wrong? Well, Docker images will be removed in 6 months and some published papers would become (much) harder to reproduce. 

### TUXML

As part of the TUXML project, we're compiling Linux kernels with different configurations. For scaling our experiments and distributing the compilations on several machines, and also for the sake of reproducibility, we have made an effort to develop an integrated environment with all tools needed (the gcc compiler, Make, but also plenty of tools such as binutils or even [bison and flex](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/Documentation/process/changes.rst?id=562f36ed28e6faa4245ea2ca1392d90ab98ebbe8)). In 2017, we toke the decision to rely on Docker. Since 2017 and up to now (2020), we toke care of releasing the actual Docker images, [considering different versions of the kernel](https://hub.docker.com/r/tuxml/tuxml/tags). We do have the Docker files for each release: it's actually a generator that produces Docker files. However, we consider it's not enough: for setting up the environment, we rely on ```apt-get``` and some updates may completely change the tools we used. As a workaround, we have separated the Docker files and the main Docker file depends on an actual Docker image that has "frozen" all versions of the tools. Like that, we don't update the system anymore with ```apt-get update```: it has already been done and we simply reuse the image (called [debiantuxml](https://hub.docker.com/r/tuxml/debiantuxml)). So what's the impact of Docker new policy on TUXML? 

* ```debiantuxml``` may well be removed in 6 months since we do not change it: the main ```tuxml``` image may be impossible to re-build (even with a Docker image)
* the tags of ```tuxml``` images may also disappear, since such releases are not supposed to change in the future: they are just artefacts for reproducibility. 

### ICPE 

In a totally different context, we have pushed tons of effort to make replicable our ICPE'2020 paper ["Sampling Effect on Performance Prediction of Configurable Systems: A Case Study"](https://github.com/diverse-project/ICPE2020). Thanks to the remarks of the program committee ([artefacts evaluation](https://icpe2020.spec.org/tracks-and-submissions/artifact-evaluation-track/)), we came across a problem with the Docker image: version of ```R``` on our Docker image was "too old". Hence, despite precise instructions, a show-stopper problem occurred and it was not the fault of the reviewers, but ours. We could identify the source of the issue and then decide to push an actual, corrected Docker image. Another related issue in this endeavor is that we actually depend on another [research work, that also relies on Docker](https://github.com/se-passau/Distance-Based_Data). We iterated a lot with the authors (thanks Christian Kaltenecker!). This experience shows two things:

* Docker is not the only problem, you may well have issues with another ecosystem and bunch of tools/libraries (```R``` here). I'm not sure only the Docker file can document your exact requirements and prevent such issues; 
* your Docker image may well depend on another Docker image: what if the original Docker image you depend on is removed? The build of your image may be impossible

We're actually seeing similar issues' patterns as with TUXML. And the pragmatic solution of releasing a proper, ready-to-use Docker image seems necessary. 

### FAMILIAR 

In yet another context, we're developing a language, called FAMILIAR, for specifying so-called feature models and performing several operations on top of them. We have published the source code and different releases in the past, taking different forms: the good-old JAR files, the ```pom.xml``` with Maven, a Web version (no longer maintained), and... a [Docker image](https://github.com/se-passau/Distance-Based_Data) (thanks Sébastien Mosser!). A script automatically builds the Docker image that eventually contains the JAR files (after a build with Maven). Nice, what could go wrong? 

Well, the Java ecosystem and Maven in particular, can disappoint you sometimes, especially when you try to rebuild your tools after some months. In our case, [Xtext has changed a bit the API](https://github.com/eclipse/xtext/issues/1373) in such a way some versions of some libraries are no longer working. FAMILIAR depends on Xtext (we are here since the beginning: 0.7.2). The fact is that Maven arbitrarily choses the versions, and if you're unlucky... the build no longer works. The "funny" thing is that the build did work on another old machine I have. Anyway, we find a [fix](https://github.com/FAMILIAR-project/familiar-language/commit/1b8dd0638bf2e6fc36b49cfb37613084f8e0338f)

Back to Maven: the push of a Docker image had the merit of "freezing" an environment that does work... It's a way to save something working, even temporary. Without the release of a Docker image, your Docker file should work, but it's not 100% sure because of some pieces not working in a given ecosystem (here, mostly Java/Maven). 



## What to do? 

Well, clearly, something before April 2021, six months after November 2020. Otherwise, the three research projects mentioned above may become non-replicable -- not impossible to replicate, but much much harder! 

An immediate alternative is to store Docker images elsewhere... on [Github packages](https://github.com/features/packages) or in a Docker registry maintained/supported by our public research institutions. But wait...

## Is it really a Docker problem? 

This short term solution is perhaps what my colleagues and I will consider, but there is a more profound issue. In fact, the new Docker policy makes me realize two things:

* the infrastructures we rely on for hosting artefacts are so fragile: Docker may well not be here in a few years... Initiatives like [softwareheritage](https://www.softwareheritage.org/) or [zenodo](https://zenodo.org/) should definitely be supported, but I'm not sure they address all kinds of reproducibility problems (like the Docker one). In general, assuming that an artefact published somewhere (on Github or Docker hub) will sustain forever is a terrible idea. Now I am thinking about all "what if" scenarios that can happen and make unavailable your artefacts: it's freaking! 
* "If the process that builds the image was reproducible, you would not need to retain images": I totally concur with this [argument](https://twitter.com/amintos/status/1300848460757950464). The source code (as hosted on  [softwareheritage](https://www.softwareheritage.org/)) can be sufficient under the conditions your build process is working. I would like to be optimistic, but at the moment "we" are not ready. Based on my experience, the problem may come from different ecosystems (```R```, ```python```, ```Java```, or simply other Docker images) that offer build tools that challenge reproducibility. Hence the problem is not only at the Docker level.



## Final remarks

Retaining Docker images seems a convenient, temporary but fragile and unsatisfactory solution. We should *push reproducibility upstream, to another level*. There are some challenges to tackle though. After all, Docker is just here to host software; many pieces of software are hard to build in a reproducible way and are in a sense the real problem. It's not new and the new policy of Docker can be seen as a last call to investigate new strategies and solutions. 

[Singularity](https://singularity.lbl.gov/) is an alternate to Docker, but won't resolve all problems listed above. The use of Nix (as an alternate to ```apt-get```) is worth trying since build packages do not have undeclared dependencies and are reproducible. However, [Nix](https://nixos.org/) does not resolve the problem of dependency management within the ecosystem of existing languages (e.g., ```Python```). 

Reproducibility is a long standing issue in science, and Docker new policy challenges current practices. I'm expecting that many scientists will have to move their Docker images in the upcoming months (otherwise many research works may simply not be reproducible). It will be an excellent excuse to re-consider the problem as a whole and think about end-to-end reproducible builds. I am not sure we are ready (did I miss something?), but it's a necessary step. As a final note, reproducibility is actually a large problem that does not only impact science, but software in general and many businesses: software-intensive organizations will have to react to the new Docker policy and perhaps change their practice to achieve reproducible builds. 









 















*(new!) I realized that some of my blog post entries are sometimes cited in academic works, so why not using the following bibtex entry?*

```bibtex
@misc{acher2020dockerreproduciblesciencechallenges,
  author = {Mathieu Acher},
  title = {Docker New Inactive Image Policy and Reproducible Science},
  year = {2020},
  month = {sep},
  howpublished = {\url{https://blog.mathieuacher.com/DockerReproducibleScienceChallenges/}},
  note = {\url{https://blog.mathieuacher.com/DockerReproducibleScienceChallenges/}}
}
```

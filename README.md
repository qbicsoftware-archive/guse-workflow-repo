# guse-workflow-repo

This is meant to be a repository for [guse](http://www.guse.hu/) workflows that are used or will be
used at qbic.


Right now the workflows are sorted in folders by the omics field they are
targeting.

**wf_config** contains json configuration files for the workflows. They are used
for visualisation. An implementation that is using them can be found here
[workflow_api](https://github.com/qbicsoftware/workflow_api).


**dropbox.json** contains a mapping for predefined openbis sample types to their
dropboxes.


Currently, we use 3 nodes in a guse workflow:

## Initialization
This one should initialize the workflow. That means set environment variables,
download reference files get files from database on the cluster, initial
external workflow, e.g. a snakemake workflow.

## Workflow
At this stage everything is initialized, ideally.
This node might be a simple script, an executable or a different workflow
(snakemake, guse sub-worklfow etc.).


## Commit
This one in (our case) commits results and logfiles to the database. From there
results and logs can be seen and used by our users.


A guse workflow has the following directory structure:
```
workflow-main-directory
├──workflow.xml
└──workflow-directory
   ├──Initialization
   │   ├──execute.bin
   │   └──inputs
   ├──Workflow
   │  ├──execute.bin
   │  └──inputs
   └──Commit
      ├──execute.bin
      └──inputs
```
**execute.bin** might be a shell script, a python script, an executable, or even a
tar ball. See [guse-workflow-scripts](https://github.com/qbicsoftware/guse-workflow-scripts).

**inputs** contain input ports, where one can usually put in parameter files or
real input files like mzML files for quality control in proteomics. However, in
our case it is strongly discouraged to use inputfiles directly because the moab
submitter of guse copies those files between server and cluster. As these files
might be quite huge it can easily become a bottleneck.
Furthermore, the input ports have to be defined in the workflow.xml.

See the [guse](http://www.guse.hu) documentation for further details.
The workflows can be submitted to a guse instance through our
[workflow_api](https://github.com/qbicsoftware/workflow_api).




TODO: what is definitely missing is a bullet-proofed versioning.

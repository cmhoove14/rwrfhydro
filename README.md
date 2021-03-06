
<!-- README.md is generated from README.Rmd. Please edit that file -->

# rwrfhydro

[![Travis-CI Build
Status](https://travis-ci.org/NCAR/rwrfhydro.png?branch=devBranch)](https://travis-ci.org/NCAR/rwrfhydro)

A community-contributed tool box for managing, analyzing, and
visualizing WRF Hydro (and HydroDART) input and output files in R.

# Purpose

Intentionally, “rwrfhydro” can be read as “our wrf hydro”. The purpose
of this R package is to focus community development of tools for working
with and analyzing data related to the WRF Hydro model. These tools are
both free and open-source, just like R, which should help make them
accessible and popular. For users new to R, several introductory
resources are listed below.

The purposes of this README are 1) to get you started using rwrfhydro
and 2) to explain the basics (and then some) of how we develop the
package so you can quickly start adding your contributions.

# Table of Contents

  - [Installing](#installing)
  - [Using](#using)
  - [Developing](#developing)
      - [Version control for collaboration:
        Github](#version-control-for-collaboration-github)
          - [Forking](#forking-and-cloning)
          - [devBranch and pull
            requests](#devbranch-and-pull-requests)  
          - [Not using Github](#not-using-github)
      - [Workflow: git, R packaging, and
        you](#workflow-git-r-packaging-and-you)
      - [Our best practices](#our-best-practices)
          - [R package best practices and code
            style](#r-package-best-practices-and-code-style)
              - [Organizing functions](#organizing-functions)
              - [R code style](#r-code-style)
              - [Packages are NOT scripts](#packages-are-not-scripts)
              - [Documentation with
                roxygen](#documentation-with-roxygen)
          - [Objects in rwrfhydro](#objects-in-rwrfhydro)
          - [Graphics](#graphics)
  - [R Package development resources](#r-package-development-resources)
  - [Introductory R resources](#introductory-r-resources)

# Installing

Installing rwrfhydro (not on [CRAN](http://cran.r-project.org/)) is
facilitated by the devtools package (on CRAN), so devtools is installed
first. The following is done for the initial install or to update the
rwrfhydro package.

``` r
install.packages("devtools")
devtools::install_github("NCAR/rwrfhydro")
```

The very first time this is run, it can take a while to install all the
package dependencies listed as “Imports” in the
[DESCRIPTION](https://github.com/NCAR/rwrfhydro/blob/master/DESCRIPTION)
file.

To check for updates once rwrfhydro is loaded run `CheckForUpdates()`.

To install other branches than master and perhaps from your own fork:

``` r
devtools::install_github("username/rwrfhydro", ref='myBranch')
```

Importantly, beta functionality can be installed using:

``` r
devtools::install_github("NCAR/rwrfhydro", ref='devBranch')
```

We are finally gaining some Windows users and have attempted to improve
portability of rwrfhydro to this system. A primary dependence of
rwrfhydro is ncdf4. This ncdf4 package binary can be installed in the
following way: First, obtain the binary from
<http://cirrus.ucsd.edu/~pierce/ncdf/>. Then in an R session

``` r
install.packages(file.choose(), repos=NULL, type = "binary")
```

R will open a window for you to choose the downloaded zip file and will
install it.

# Using

After the one-time install or any subsequent re-installs/updates, simply
load the rwrfhydro library in an R session:

``` r
library(rwrfhydro)
```

and now the package (namespace) is available.

[*Online
vignettes*](https://github.com/NCAR/rwrfhydro/blob/master/vignettes.Rmd)
(or in R `browseVignettes("rwrfhydro")`) are probably the easiest way to
get in-depth, thematic overviews of rwrfhydro functionality.

To get package metadata and a listing of functions:
`library(help=rwrfhydro)`. Just the functions:
`ls('package:rwrfhydro')`. For specific functionality see function help
(e.g. `?VisualizeDomain` or `help(VisualizeDomain)`).

# Developing and bug reports

Bugs are to be reported
[here](https://github.com/NCAR/rwrfhydro/issues). If you want to help
solve bugs and fixes into the code, please continue reading about
developing.

There are four main aspects of developing the code base and
contributing:

  - Version control for collaboration: Not terribly interesting but
    incredibly useful. For those new to the Git/Github process, it can
    be a bit daunting. Please contact us for some help, we do want to
    get your useful code into the main repository\!

  - R Packaging: Again, not very interesting, but critical for creating
    the extrememly useful nature of R packages. Fortunately, the
    `devtools` package simplifies packaging tremendously and so figures
    prominently in the development process we sketch below. The main
    details have been sorted out, contributing new functions is
    generally fairly easy.

  - Our best practices: This ranges from fundamental to fussy.

  - Getting in touch with the community: Lots of aspects of this tool
    are under active development. Dont duplicate efforts, extend them\!
    We will establish a site for communicating development tasks and
    statuses on the github wiki.

R packageing and some version controll are treated by [Hadley Wickham’s
book on R Packages](http://r-pkgs.had.co.nz/). Specific sections of this
book are linked below. Further resources on R package development are
listed at the end of this page.

## Version control for collaboration: Github

Instead of going straight to developing, we recommend that you install
rwrfhydro using `devtools::install_github('NCAR/rwrfhydro')` first,
because this streamlines the installation of package dependencies. Note
that `devtools::install_github('NCAR/rwrfhydro')` installs rwrfhydro
into you default library path (`.libPaths()[1]`) and that the source
code is not included.

The very best way to obtain and edit the source is to “fork” rwrfhydro
on github and then clone *your* the repository to your machine(s) of
choice. You edit your fork and, when it’s ready, you submit a pull
request to get the changes back to the main (upstream) fork of
rwrfhydro. More details are provided below. Your cloned git repository
is not in your default R library path (`.libPaths()[1]`), but somewhere
else where you choose to keep your development code. However, devtools
allows you to build your development package into your library path.
This means that after you add some code locally, you can
`library(rwrfhydro)` from other R sessions on that machine with your
changes appearing in the package. The basic use of devtools is outlined
below. It greatly stream lines all aspects of developing R packages and
is highly recommended. Particularly, it make is easy to go from github
or local changes to an R package.

### Forking and cloning

Please fork the repository to contribute. A fork is a separate copy of
the main repository on which you have write permissions. Note that you
do not have write permissions on any other fork of the repository.
[Forking is trivial in
Github](https://help.github.com/articles/fork-a-repo/). You have to have
a free (for open-source repositories) account to fork on github.

Next you’ll clone *your* fork to your local computer and you’ll to
interact between your forked repository on github, which is called
“origin”. The repo “NCAR/rwrfhydro” is known as the “upstream” fork.
This is the “official” repo. It’s also called upstream because changes
to it should always flow to all other repos so that they can easily sync
their separate changes back to it. Keep your fork sync’d with upstream
as much/often as possible to avoid painful merges, github notifies
downstream forks of changes to upstream.

Because you dont have write permissions to “upstream” (or any other
fork), you have to request that your changes be pulled upstream. This is
done via a pull request on github (website). We give some tips below and
give a general overview of forking on github in [this
document](https://docs.google.com/document/d/1DxsViogPdA0uObHgNx4YFKd4ClC-m9UFcX0rO-ZJTY0/edit?usp=sharing).

### devBranch and pull requests

We maintain two main branches or rwrfhydro: master and devBranch. You
should *never work on the master branch*. All changes have to pass
through the devBranch before going in to the master and this is
controlled by the package maintainer. Therefore, devBranch is where your
[pull requests](https://help.github.com/articles/using-pull-requests/)
will go. Other barnches on your fork are up to you. How you get your
code into your fork’s devBranch is your choice. One suggestion is to
work on your personal branch. Then when various files are ready to be
contributed to `devBranch`, you first do `git checkout devBranch` then
followed by `git checkout myBranch -- path/to/file` for each file you
desire to copy from `myBranch` into `devBranch`. Finally the `git add`
and `git commit` formally put these files into `devBranch`. Some more
details on using git are provided in the
[workflow](#workflow-git-r-packaging-and-you) overview below.

### Not using Github.

This is not recommended, but might be possible. It will certainly hinder
you interaction with the upstream repo.

## Travis-CI and `R CMD check`

The rwrfhydro repo is configured to build on a third-party virtual linux
machine with every push or pull request to the master or devBranch
branches. This service is known as Travis-CI (continuous integration).
This means your pull requests are automatically checked by `R CMD
check`, this keeps errors from creeping into the upstream code. There
are a variety of hurdles to getting code to build on Travis-CI,
including installing requisite system and R packages, which can be
challenging but worth it for the debugging provided by automated builds
in conjunction with `R CMD check`.

`R CMD check` accepts a variety of arguments. Ultimately, it 1) checks
the source for consistency including across platforms (Windows, OSX,
linux), 2) runs all specified code tests, essentially regression tests,
3) runs all the examples provided in the documentation, and 4) builds
all the vignettes. Currently, we are skipping vignette building until we
can streamline several of these.

You can configure your own fork to build on Travis-CI and you can push
frequently to check for errors. This is nearly identical to (slightly
more stringent than) running `devtools::check()`, but all you have to do
is push your commits.

## Workflow: git, R Packaging, and you

Workflow is approximately this:

  - Fork project on github

  - Clone *your* github fork to your local machine(s)

  - Set the upstream repo (`git remote add upstream
    https://github.com/NCAR/rwrfhydro` - different syntax for ssh
    access)

  - Checkout your devBranch (`git checkout devBranch`)

  - Development cycles:
    
      - Optional: Create a topic branch off of devBranch in git (e.g.
        `git checkout -b myBranch`) and push this to github (`git push
        origin myBranch`).
      - Write code (in these dirs: R/, NAMESPACE, src/, data/, inst/).
      - Write documentation (in these dirs: man/, vignettes/).
      - Write tests (in this dir: test/).
      - Document and check with devtools: `devtools::document();
        devtools::check_man(); devtools::check()`
      - Commit to your branch with git. (`git commit -am 'Some cool
        features were needed.'`)
      - Push to github (`git push origin branch`). If Travis-CI is
        configured, this can trigger an `R CMD check` on Travis.

  - To get code back to the main reposiory/fork:
    
      - If on myBranch: You probably want to mege devBranch into
        myBranch: `git pull upstream devBranch`.
      - If on myBranch: `checkout devBranch`. The either wholsale merge
        your work from origin (`git pull origin myBranch`) or cherry
        pick files (`git check out myBranch -- path/to/file` for eac
        file) it into devBranch.
      - if on myBranch: commit, `git commit -am'Fixes and new
        features'`.
      - If not previously on myBranch and did not do a wholesale merge
        in previous step: Sync with the “upstream” devBranch: [See
        here.](https://help.github.com/articles/syncing-a-fork/) If
        upstream repo is set: `git pull upstream devBranch`.
      - Push your devBranch to your fork on github: `git push origin
        devBranch`.
      - Submit a [pull request on
        github](https://help.github.com/articles/using-pull-requests/)
        on devBranch to upstream (NCAR/rwrfhydro).

## Our best practices

### R package best practices and code style

<http://r-pkgs.had.co.nz/r.html>

#### Organizing functions

<http://r-pkgs.had.co.nz/r.html#r-organising>

  - Do NOT put all functions in a single file, nor each in their own
    file. Functions should be grouped by files and may occasionally need
    moved to new or different files as new functions are written.
  - File names end in .R and are all lowercase with \_ used to separate
    words. (All lowercase (except the .R) helps ensure compatibility
    with Windows developers.)

#### R code style

<http://r-pkgs.had.co.nz/r.html#style>

  - Generally follow Google’s R style guide with preference for
    variableName (first-lower camel case) over variable.name (period
    distinction). Note that functions are first-upper camel case,
    e.g. FunctionName.
    <https://google-styleguide.googlecode.com/svn/trunk/Rguide.xml>
  - Variables are nouns. Functions are verbs.
  - Lots of other style considerations to learn: indents, braces, line
    length, assignment, comment style.

#### Packages are NOT scripts

<http://r-pkgs.had.co.nz/r.html#r-differences>

  - Don’t use library() or require(). Use the DESCRIPTION to specify
    your package’s requirements.
  - Use package::function() to use function from external packages. Make
    sure the package and version are listed in DESCRIPTION.
  - Never use source() to load code from a file. Rely on
    devtools::load\_all() to automatically source all files in R/.
  - Don’t modify global options() or graphics par(). Put state changing
    operations in functions that the user can call when they want.
  - Don’t save files to disk with write(), write.csv(), or saveRDS().
    Use data/ to cache important data files.

#### Documentation with roxygen

<http://r-pkgs.had.co.nz/man.html>

Once you get used to this, you will love writing documentation as you go
for your R functions.

  - Roxygen comments start with \#’ and come before a function. All the
    roxygen lines preceding a function are called ablock. Each line
    should be wrapped in the same way as your code, normally at 80
    characters.
  - Blocks are broken up into tags, which look like @tagName details.
    The content of a tag extends from the end of the tag name to the
    start of the next tag (or the end of the block). Because @ has a
    special meaning in roxygen, you need to write @@ if you want to add
    a literal @ to the documentation (this is mostly important for email
    addresses and for accessing slots of S4 objects).
  - Each block includes some text before the first tag. This is called
    the introduction, and is parsed specially:
  - The first sentence becomes the title of the documentation. That’s
    what you see when you look at help(package = mypackage) and is shown
    at the top of each help file. It should fit on one line, be written
    in sentence case, and end in a full stop.
  - The second paragraph is the description: this comes first in the
    documentation and should briefly describe what the function does.
  - The third and subsequent paragraphs go into the details: this is a
    (often long) section that is shown after the argument description
    and should go into detail about how the function works.
  - All objects must have a title and description. Details are optional.
  - GetPkgMeta: NOTE that this function only works for packages *not*
    installed by devtools::load\_all(). The function analzes the
    @keywords and @concepts tags supplied in the roxygen documentation
    to categorize and relate functions and objects in rwrfhydro (or any
    other package). Keywords follow R conventions (see this obscure link
    which I need to find). Concepts are customized to rwrfhydro. The
    current list of keywords and concepts can be gathered from the
    latest version of devBranch, using the following command, which
    gives as of this writing:

<!-- end list -->

``` r
> GetPkgMeta(listMetaOnly=TRUE)
-----------------------------------
rwrfhydro concepts
-----------------------------------
Ameriflux
DART
data
dataAnalysis
dataGet
dataMgmt
geospatial
getData
GHCN
modelEval
MODIS
ncdf
nudging
plot
SNODAS
SNOTEL
Streamflow
usgs
usgsStreamObs

-----------------------------------
rwrfhydro keywords
-----------------------------------
data
database
hplot
internal
IO
manip
smooth
ts
univar
utilities
```

### Objects in rwrfhydro

We will probably need to develop some s3 classes or reuse some from
other packages. List of possible objects: gaugePts object for organizing
“frxst points”, both locations and data.

### Graphics

We need to resolve if we are going to use base graphics or ggplot or
both. I’m leaning towards both. Not all plotting routines have to always
be available for a given function, but I think that both will probably
develop over time.

Because ggplot has a big learning curve, we can return closures which 1)
provide tweakability for basic things to be adjusted in the plot make
the plot when called, 2) which return the basic ggplot object which can
then also be extended with ggplot commands. I made an example of this in
VisualizeDomain.R for ggmap/ggplot
    objects.

# R Package development resources

  - <http://hilaryparker.com/2014/04/29/writing-an-r-package-from-scratch/>
  - <http://r-pkgs.had.co.nz/>
  - <http://cran.r-project.org/doc/contrib/Leisch-CreatingPackages.pdf>
  - <http://portal.stats.ox.ac.uk/userdata/ruth/APTS2012/Rcourse10.pdf>

# Introductory R resources (somewhat random)

  - \[My introduction to R: multiple resources but, sorry, the video
    link is broken.\] (<https://nex.nasa.gov/nex/resources/118/>)
  - \[My R cheat sheet (also availabile in LaTex inthe above link).\]
    (<https://nex.nasa.gov/nex/static/media/other/R-refcard_2.pdf>)
  - \[The popular YouTube serires on R by Roger Peng.\]
    (<https://www.youtube.com/user/rdpeng>)
  - \[<https://www.datacamp.com/courses/free-introduction-to-r>\]
    (<https://www.datacamp.com/courses/free-introduction-to-r>)
  - \[<http://cran.r-project.org/doc/contrib/Torfs+Brauer-Short-R-Intro.pdf>\]
    (<http://cran.r-project.org/doc/contrib/Torfs+Brauer-Short-R-Intro.pdf>)
  - \[<http://cran.r-project.org/doc/manuals/R-intro.pdf>\]
    (<http://cran.r-project.org/doc/manuals/R-intro.pdf>)

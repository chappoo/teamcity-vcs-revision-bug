# Steps to reproduce

- Clone this repository:
    -     git clone https://github.com/chappoo/teamcity-vcs-revision-bug.git
- From Terminal / Editor, browse to the checkout directory (assumed throughout this process)
- Bring up the TeamCity environment:
    -     docker-compose up -d
- Wait a few moments for the containers to start
- Browse to the TeamCity website: http://localhost:8111
- Login using: `admin` / `admin`

> The [Master build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_Master) configuration is a top-level build, that snapshot depends on the [ProjectA build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_ProjectA) and the [ProjectB build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_ProjectB), each of which snapshot depend on the [ProjectC build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_ProjectC) respectively. 

> Open the [build chain](http://localhost:8111/viewChain.html?chainId=bt5&selectedBuildTypeId=bt5&contextProjectId=VcsRevisionBug) for a more complete graphical view

> Note that all the builds are configured with VCS Checkout Rules that restrict the files monitored to the respective area in this Git repository, so ProjectA only monitors ProjectA files, and so on.

- Run the [Master build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_Master).  All builds should trigger

## Bug:

- From a Terminal / Editor, create a branch and commit a change to:
    - `ProjectA/README.md`
- TeamCity [Master build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_Master) trigger will activate, and calculate that all builds in the chain should run

## Expectation:

- [ProjectA build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_ProjectA) and [Master build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_Master) should run in order, with [ProjectB build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_ProjectB) and [ProjectC build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_ProjectC) satisfied by the existing builds from the `master` branch revision  

## Workaround:

There is a workaround for this:

- Checkout `master` branch
- From a Terminal / Editor, commit changes directly to `master` for:
    - `ProjectA/README.md`
    - `ProjectB/README.md`
    - `ProjectC/README.md`
- [Master build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_Master) chain will trigger as expected for all builds

Repeat the test

- From a Terminal / Editor, now create a branch from `master` and commit a change to:
    - `ProjectA/README.md`

The build chain will now correctly execute, in order:
- [ProjectA build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_ProjectA)
- [Master build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_Master)

## Comments

This behaviour seems to display some consistency with the symptoms explained in [this issue](https://youtrack.jetbrains.net/issue/TW-10084), and [this issue](https://youtrack.jetbrains.com/issue/TW-47099).  However you'll notice that in the above recreation steps, the VCS root was not changed, and is still symptomatic.

The best outcome here would of course be to fix whatever the root cause may be and ensure consistent branch builds, but just diagnosing this bug incurred unexpected cost, primarily due to navigating the obscurity of what was going on and forming the workaround.  Combining this with the fact this issue seems to have been around for years (reported in some form or another), it would probably be almost as beneficial to update the documentation to describe the current behaviour clearly for VCS Root / Checkout Rules.
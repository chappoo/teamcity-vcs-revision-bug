# Steps to reproduce

- Clone this repository:
    -     git clone git://uri/
- From Terminal / Editor, browse to the checkout directory (assumed throughout this process)
- Bring up the TeamCity environment:
    -     docker-compose up -d
- Wait a few moments for the containers to start
- Browse to the TeamCity website: http://localhost:8111
- Login using: `admin` / `admin`

> The [Master build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_Master) configuration is a top-level build, which snapshot depends on the [ProjectA build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_ProjectA) and the [ProjectB build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_ProjectB), each of which snapshot depend on the [ProjectC build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_ProjectC) respectively. 

> Open the [build chain](http://localhost:8111/viewChain.html?chainId=bt5&selectedBuildTypeId=bt5&contextProjectId=VcsRevisionBug) for a graphical view

- Run the [Master build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_Master); all builds should trigger

Test:

- From a Terminal / Editor, create a branch and commit a change to:
    - `ProjectA/README.md`

Bug:

- TeamCity [Master build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_Master) trigger will activate, and calculate that all builds in the chain should run

Expectation:

- [ProjectA build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_ProjectA) and [Master build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_Master) should run in order, with [ProjectB build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_ProjectB) and [ProjectC build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_ProjectC) satisfied by the existing builds from the `master` branch revision  

Workaround:

There is a workaround for this:

- Checkout `master` branch
- From a Terminal / Editor, commit changes (direct to `master`) to:
    - `ProjectA/README.md`
    - `ProjectB/README.md`
    - `ProjectC/README.md`
- [Master build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_Master) chain will trigger as expected for all builds

Repeat the test

- From a Terminal / Editor, create a branch and commit a change to:
    - `ProjectA/README.md`

The build chain will now correctly execute just [ProjectA build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_ProjectA) and [Master build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_Master)
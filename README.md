# Steps to reproduce

- Clone this repository:
    -     git clone git://uri/
- From Terminal / Editor, browse to the checkout directory (assumed throughout this process)
- Bring up the TeamCity environment:
    -     docker-compose up -d
- Browse to the TeamCity website: http://localhost:8111
- Login using: `admin` / `admin`

> The [Master build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_Master) configuration is a top-level build, which snapshot depends on the [ProjectA build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_ProjectA) and the [ProjectB build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_ProjectB), each of which snapshot depend on the [ProjectC build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_ProjectC) respectively. 

> Open the [build chain](http://localhost:8111/viewChain.html?chainId=bt5&selectedBuildTypeId=bt5&contextProjectId=VcsRevisionBug) for a graphical view

- Run the [Master build](http://localhost:8111/viewType.html?buildTypeId=VcsRevisionBug_Master)

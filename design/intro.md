# design concerns

**"messy = re-write, organized = refactor"**

It's supprising how many times you can re-structure your code just to have the same functionality, but in a better way. I consider this wasted project time but valuable to develop skills. Hard to follow code is eventually rewritten. That does not mean its bad if it did its job well. It's just the nature of humans writing code.

You have a project with no idea of how to solve it yet, but to prevent a rewrite or a feeling of this code sucks, you need to solve other problems that you will face later depending on how big it is. So start asking these questions.

## how big will it be ?

**"directory structures can help organize logic structures."**



1. hello world or one file tests need no structure externally.
    1. except virtual env. read [virtual env considerations](#)
1. A one file utility may need one of tests, config, packaging
1. Consider how to make the directory structues to scale if needed:
    1. organizing source code into groups. what directory to put them in for the system:
        1. utility scripts (CLI) like db migration, data integerity, process managment
        1. data services dealing with caching, messaging, joining data, webAPI etc 
        1. interfaces to external resources, database, gdrive, mail, etc
        1. user interfaces, shell, web, documentation, email etc
    1. configs (files and environments)
    1. tests code
    1. how to handle system uitilities like cron, docker or podman, systemd, supervisord, pypi packaging

# recomendations

Use a basic modular pattern for the directory structure. If it grows you do less restructuring no matter how big it gets.

Example template: add subtract what you need

from this
```
foobar-project/
│── foobar_cli/         # command line tools.
|   │──__init__.py
│   │── env.py  
│   │── actions.py  
│
│── README.md 

```

to this
```
foobar-project/
│── foobar_backend/     # data services 
│   │── __init__.py
│   │── env.py           
│   │── core.py
│   │── models.py
│   │── fastAPI.py      # web API
│
│── foobar_ui/          # UI tools 
│   │── __init__.py
│   │── env.py           
│   │── fastUI.py      
│
│── foobar_cli/         # command line tools.
|   │──__init__.py
│   │── env.py           
|   │──server.py
|   │──client.py
│
│── foobar_common/      # msg protocol / encoders / data helpers etc
|   │──__init__.py
|   │──schemas.py       
│
│── configs/            # config  
|   │──foobar.ini       
|   │──systemd.cfg      # linux system startup config
|   │──supervisor.conf  # win/linux/mac startup system config
|
│── README.md           # links to docs/intro.md etc..
│── docs/
│   │── intro.md
|
│── tests/
|   │──test_services.py
|   │──test_ui.py
|   │──test_cli.py
|
│── pytest.toml        # controls the testing paths and output
│── pyproject.toml     # Modern build system config

```
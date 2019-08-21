# cfg-maxiv-gitlabci

## Description

YML templates for GitLab CI / CD

## Usage


Used as a base reference for all (at least most) of our gitlab projects.

In order to use this template you need to create a `.gitlab-ci.yml` inside your project
repository. As and example you can use the following code:

### Python

```yml
include:
  - project: 'kits-maxiv/cfg-maxiv-gitlabci'
    ref: master
    file: '/.python-ci.yml'

```
You can also use `FPM_FLAGS` variables to specify others fpms flags.

The current list of build job supported:
* build el6 python 27
* build el6 python 34
* build el6 python 36
* build el7 python 27
* build el7 python 34
* build el7 python 36

By default python-ci will publish all builds.

FPM does not use the setup.cfg, configuration file, to get the build requires and package dependencies.
To define the dependencies (build and installation) you should update your setup.py like this:

```python
from setuptools import setup, find_packages


def main():
    """Main method collecting all the parameters to setup."""
    name = "tangods-example"

    version = "1.0.0"

    description = "Setup.py example file."

    author = "KITS - Controls"

    author_email = "KITS@maxiv.lu.se"

    license = "GPLv3"

    url = "http://www.maxiv.lu.se"

    packages = find_packages()

    provides = ["example"]

    requires = ['example_pkg1', 'example_pkg2']

    install_requires = ['example_pkg1', 'example_pkg3']

    entry_points={'console_scripts': ['Example = example:main']}

    setup(
        name=name,
        version=version,
        description=description,
        author=author,
        author_email=author_email,
        license=license,
        url=url,
        packages=packages,
        requires=requires,
        install_requires=install_requires,
        entry_points=entry_points,
    )

if __name__ == "__main__":
    main()
```

By default FPM will automatically add `python` (variate with python version) as a prefix to your requires list, install-requires list and package name.

You can also decide to disable some combinations of builds by overiding the build job and by defining a variable UNSUPPORTED, i.e:

```yml
build el7 python 36:
  variables:
    UNSUPPORTED: 'True'

```

### Python Tango Device

```yml
include:
  - project: 'kits-maxiv/cfg-maxiv-gitlabci'
    ref: master
    file: '/.pythonds-ci.yml'

build el7 pythonds 34:
  variables:
    PACKAGE_NAME: 'tangods-example'

```
For tango devices you will need to define the `PACKAGE_NAME` variable. This variable will be the name of your RPM package.

You can also use `FPM_FLAGS` variables to specify others fpms flags.

The current list of build job supported:
* build el6 pythonds 27
* build el6 pythonds 34
* build el6 pythonds 36
* build el7 pythonds 27
* build el7 pythonds 34
* build el7 pythonds 36

By default pythonds-ci will publish just this build:
* build el7 pythonds 34

To select what python build you want you can overiding the build job and by defining a variable DEPLOY, i.e:

```yml
build el7 pythonds 27:
  variables:
    DEPLOY: 'True'
    PACKAGE_NAME: 'tangods-example'

build el7 pythonds 34:
  variables:
    DEPLOY: 'False'

```
You can also decide to disable some combinaison of builds by overiding the build job and by defining a variable UNSUPPORTED, i.e:

```yml
build el7 pythonds 36:
  variables:
    UNSUPPORTED: 'True'

```


### C++

```yml
include:
  - project: 'kits-maxiv/cfg-maxiv-gitlabci'
    file: '/.cpp-ci.yml'
```

In order to use this template you need a .spec file in order for maxpkg to build and package your project.

You can also decide to disable some combinaison of builds by overiding the build job and by defining a variable UNSUPPORTED, i.e:

```yml
build el7 i386:
  variables:
    UNSUPPORTED: 'True'

```
The current list of build job supported:
* build el7 x86_64
* build el6 x86_64
* build el6 i386

The current list of build job but disable by default:
* build el7 i386

#### Define more platform to build
In case you want to add new platform you should add new build job if the current template is compatible. Also don't forget to add as many build as many publishing triggers. The publish job is generic enough for the RPM based system.

#### Links

Gitlab CI documentation:

* https://docs.gitlab.com/ee/ci/

FPM repo:

* https://github.com/jordansissel/fpm

FPM wiki with all tags:

* https://github.com/jordansissel/fpm/wiki

maxpkg repo:

* https://gitlab.maxiv.lu.se/kits-maxiv/app-maxiv-maxpkg

# Authors

KIST SW group

kitscontrols@maxiv.lu.se

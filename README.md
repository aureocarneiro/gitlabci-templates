# cfg-maxiv-gitlabci

## Description

YML templates for GitLab CI / CD

## Usage

Used as a base reference for all (at least most) of our gitlab projects.

In order to use this template you need to create a `.gitlab-ci.yml` inside your project 
repository. As and example you can use the following code:

```yml
include:
  - project: 'kits-maxiv/cfg-maxiv-gitlabci'
    ref: master
    file: '/base-ci.yml'

stages:
  - Trigger
  - Build
  - PublishRPM
```



In order to use this template you need to create a `Makefile` inside your project 
repository. As and example you can use the following code:

```make
# Makefile for building the package, triggered buy the gitlab-ci
clean:
	rm ./*.*.rpm -f
	rm -rf build/
	rm -rf *.egg-info/

build:
	fpm -s python -t rpm --python-bin python --name AlarmPLC --rpm-dist el7 --verbose --iteration "1" .

test: build
	sudo yum install -y *.el7.noarch.rpm
```

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

By default FPM will automatically add `python` as a prefix to your requires list, install-requires list and package name.
You can manage that using tags in your FPM command.




## Authors

KIST SW group

kitscontrols@maxiv.lu.se


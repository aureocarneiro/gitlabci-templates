# cfg-maxiv-gitlabci

## Description

YML templates for GitLab CI / CD

## Usage

Used as a base reference for all (atleast most) of our gitlab projects.

## Authors

KIST SW group

kitscontrols@maxiv.lu.se

## Makefile for base-ci.yml

```
# Example of Makefile to build python fpm project
clean:
    rm ./*.*.rpm -f
    rm -rf build/
    rm -rf *.egg-info/

build: clean
    fpm -s python -t rpm --python-bin python2.7 --no-python-fix-name --no-python-downcase-dependencies  --no-python-fix-dependencies --python-package-name-prefix python --rpm-dist el7 .

test: build
    sudo yum install -y *.el7.noarch.rpm
```



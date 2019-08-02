# cfg-maxiv-gitlabci

## Description

YML templates for GitLab CI / CD

## Usage

Used as a base reference for all (at least most) of our gitlab projects.

In order to use this template you need to create a Makefile inside your project 
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

## Authors

KIST SW group

kitscontrols@maxiv.lu.se


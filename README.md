#Mozilla CommitBuilder
The [mozcommitbuilder](http://samliu.github.com/mozcommitbuilder) package is a generic builder library for Mozilla Firefox. Interactive regression finding over changesets using local builds.

#Summary
Downloads the mozilla-central mercurial repository to a local cache and allows operations on that trunk
including interactive regression finding over changeset ranges and building particular revisions of firefox and running them.
In particular, this module was designed for regression hunting and to provide a flexible builder for firefox.

#Dependencies
Mercurial. A correctly-configured mozconfig dotfile is optional. By default, mozcommitbuilder generates its own mozconfig, but you can pass in your own through params/cli if the script fails to build correctly or if you need other configurations..

#Install
mozcommitbuilder is available on pypi, so you can install it with easy_install or pip. The recommended method is pip.

	sudo pip install mozcommitbuilder --upgrade

#Usage
## Command-line Interface

```python
#This is an example of a command to do interactive bisection between
#changesets (you can use the full changeset hash or short numerical id)
mozcommitbuilder -g 70168 -b 70180
```
```python
#If you are on OSX or Linux, this will compile with 8 cores. Multi-core
#compiliation on windows doesn't work, the parameter will be ignored.
mozcommitbuilder -g 70168 -b 70180 -c 8
```

```python
#This will delete the cached trunk and download a new one, then build
#revision 70168 with 8 cores
mozcommitbuilder -f --single=70168 -c 8
```

```python
#This will delete the cached trunk and download a new one, then build
#revision 70168 with 8 cores, then start firefox on completion.
mozcommitbuilder -f -e --single=70168 -c 8
```

## Python API

```python
#Instantiation
from mozcommitbuilder import Builder
commitBuilder = Builder()
```

```python
#Remote Trunk Operations:
commitBuilder.getTrunk() # fetches a local trunk
commitBuilder.getBinary("70168") # Builds binary from revision,
                                 # moves into the cache directory
                                 # returns the path to it
```

```python
#Repository information:

#Returns tip's changest number
mostRecentChangeset = commitBuilder.getTip()

#Returns first changeset from a given date
firstChangesetFromDay = commitBuilder.changesetFromDay(date="2011-06-31")

#Returns last changeset from a given date
lastChangesetFromDay = commitBuilder.changesetFromDay(date="2011-06-31",oldest=False)
```


#Other
For bisection: if the range of commits spans more than a day, try [mozregression](http://harthur.github.com/mozregression)

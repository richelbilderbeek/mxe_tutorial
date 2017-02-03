# Troubleshooting

## `sudo apt-get install gcov-5 failed and exited with 100 during .`

Or in full:

```
The command "sudo -E apt-get -yq --no-install-suggests --no-install-recommends --force-yes install gcov-5" failed and exited with 100 during .
```

It means Travis-CI cannot find the package 'gcov-5' in the 'addons' section.

Comment out the addons section and add this to the Travis script:

```
apt-cache search "gcov" | egrep "^gcov"
```

One can then observe that `gcov` is absent. It is part of g++.

## Cannot find the correct version of a package

Comment out the addons section and add this to the Travis script:

```
 - apt-cache search "g++" | egrep "^gcc"
 - apt-cache search "g++" | egrep "^g\+\+"
 - apt-cache search "gcov" | egrep "^gcov"
 - apt-cache search "libboost"| egrep "^libboost"
```

This will cause Travis to search the aptitude packages.

## `fatal error: Rcpp.h: No such file or directory`

Add these line to the `.travis.yml` file to find Rcpp.h:

```
after_failure:
 # fatal error: Rcpp.h: No such file or directory
 - find / -name 'Rcpp.h'
```

You can then add the folder found to the INCLUDEPATHS of the Qt Create project file.


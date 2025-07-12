
**Links**

- Article series in Fedora Magazine:
	- [RPM packages explained](https://fedoramagazine.org/rpm-packages-explained/)
	- [How RPM packages are made: the source RPM](https://fedoramagazine.org/how-rpm-packages-are-made-the-source-rpm/)
	- [How RPM packages are made: the spec file](https://fedoramagazine.org/how-rpm-packages-are-made-the-spec-file/)

---

To get the source rpm package that generated a given binary rpm package:

```
dnf repoquery --source <BINARY_PKG_NAME>
```

To download a given source rpm package:

```
dnf download --source <SOURCE_PKG_NAME>
```

All the tools necessary to build rpm packages are provided by the package:

To install the build dependencies of a given source rpm package (needs root privileges):

```
sudo dnf builddep <PATH_TO_SRC_RPM_FILE>
```


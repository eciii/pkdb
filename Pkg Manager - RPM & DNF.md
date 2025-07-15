
**Links**

- Article series in Fedora Magazine:
	- [RPM packages explained](https://fedoramagazine.org/rpm-packages-explained/)
	- [How RPM packages are made: the source RPM](https://fedoramagazine.org/how-rpm-packages-are-made-the-source-rpm/)
	- [How RPM packages are made: the spec file](https://fedoramagazine.org/how-rpm-packages-are-made-the-spec-file/)
- https://rpm-packaging-guide.github.io/
- Official RPM manual: https://rpm.org/docs/4.20.x/manual/

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

---

**Initial overview of RPM-related packages in CentOS 9**

The `rpm` source package produce several binary packages. Some of them are:

- `rpm`: The main package. Depends on:
	- `rpm-libs`: Main shared libraries.
- `rpm-build`: Tools to build RPM packages. Depends on `rpm` and `rpm-libs` and:
	- `rpm-build-libs`: Shared libraries for building packages.
- `rpm-sign`: Support for digitally signing RPM packages. Depends on `rpm-libs` and:
	- `rpm-sign-libs`: Shared libraries for signing packages.
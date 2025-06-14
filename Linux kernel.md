#OSS_Project_Overview

The source code of the Linux kernel and other related projects is located in a [[cgit]] website at [https://git.kernel.org/pub/scm/](https://git.kernel.org/pub/scm/). The "/pub/scm/" part in the URL serves as the root of the folder structure controlled by cgit and is optional for the homepage. The actual official source code of the Linux kernel is located under `/kernel/git/torvalds/linux.git`.

---

**Useful git log commands**

Display all the history in graph form: `git log --all --oneline --graph`.

Display the list of merge commits on the master branch between the `v6.15` and `v.6.16-rc1` tags sorted by the repository where the merge was pulled from.

```
git log --format='%H;%s' --first-parent --merges v6.15..v6.16-rc1 \
| sed '
	s/;Merge tag ./;/
	s/. of /;/
	s/\(.*\);\(.*\);\(.*\)/\1;\3;\2/' \
| sort -k2,2 -t';' \
| column -ts';' \
| less
```

Display the commits merged on merge commit `27605c8c0f69` in graph form

```
git log \
	--oneline --graph --right-only --boundary \
	$(git show --format="%P" 27605c8c0f69 | sed 's/ /.../')
```

_Sort of formal theory_

A _base branch_ receives commits from a _tributary repository_ whenever there is a _remote merge commit_, that is, a merge commit of the form:

- `Merge tag 'TAG' of proto://some.domain.tld/some/path` or
- `Merge branch 'BRANCH' of proto://some.domain.tld/some/path`

on the base branch (the tributary repository being `proto://some.domain.tld/some/path`).

The collection of received commits on a remote merge commit is called the _contributed commit set_ of that specific remote merge commit. A contributed commit set might also display the same "base/tributaries" pattern.

A tributary repository can contribute commits at one or more remote merge commits.

The _proper commits_ of a base branch are those commits that don't come from tributary repositories. The tributary merge commits themselves also count as proper commits.

The maintainers of a tributary repository are the commiters of the proper commits of all the commit sets contributed by that repository.
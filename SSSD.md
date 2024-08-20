Official website: [https://sssd.io](https://sssd.io/)

Documentation: The official website holds the main documentation. There seems to be also a very good documentation of SSSD [here](https://docs.pagure.org/sssd.sssd/index.html).

---

Useful related commands:

```
id -u xyz               # Shows the actual uid of user
sssctl user-checks xyz  # Shows what the sssd daemon knows about the user
sssctl user-show xyz    # Shows info about the sssd cache status of user
sss_cache -u xyz        # Clears the sssd cache entry of user
sss_override user-add xyz -u 123  # Creates a local override of uid of user
nscd -i passwd          # Clears (invalidates) the password cache table of nscd
                        # (nscd provides yet another cache for uids)
```

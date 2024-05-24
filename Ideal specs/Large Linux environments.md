**Preamble**

- An IT Services (ITS) project is a project that provides one or more IT services to a given client.
- To provide those services the project team needs to setup and continuously manage an _IT System_ that consists of software running on physical and/or virtual infrastructure.
- An ITS project is successful if the services are provided with _High Reliability_.
- The concept of High Reliability is left vague on purpose because each ITS project should determine a concrete and measurable implementation of it.
- However there are a number of traits (kind of necessary conditions) that are usually associated with High Reliability:
	- Stability: when the number of incidents and their severity is maintained as low as possible.
	- Automation: when the number of tasks that have to be performed manually is as low as possible.
	- Adaptability:  when the architecture of the IT System is continually reviewed and improved.

**Lessons on how to manage a large Linux environment**

- No shared admin user. Everyone uses their own user instead.
- SSH authentication using kerberos. No passwords, no public/private key-pairs, no ssh certificates (revocation is difficult with ssh certificates).
- Service hosts should be considered cattle, not pets. Thus they are better deployed as containers.
- Administrative hosts should
	- have regular unattended security updates
- Administrative cronjobs:
	- should never be configured in a private users' crontab. Use a shared system user's crontab instead (but such system user should not be able to login interactively, among other restrictions).
	- should always rise warning/alert events on a central incident management system whenever they fail. No emails.
	- should always log what they do on a central logging system.
- Client's CA cert should always be installed in the system's cert pool
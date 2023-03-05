Steps 1A, 1B and 1C are independet of each other and can be executed in a different order

- 1A: Setup the local repository
  * Use `git init` to initialize a local git repository either from an existing project or from zero.
  * If the default branch is called "master" rename it to "main" using the command:

        git branch -m main

  * Configure the repo's user name and email. Make sure to use the same username and email configured in you GitHub account.

        git config user.email <username@domain.tld>
        git config user.name <username>

  * Make sure to have at least one commit in the repo's history.
  * Setup the default divergence reconciliation mechanism with one of the following commands:

        git config pull.rebase false  # merge
        git config pull.rebase true   # rebase
        git config pull.ff only       # fast-forward only

- 1B: Setup the GitHub repository
  * Create a new repository in github with a README and License. Make sure that the default branch is called "main".
- 1C: Configure SSH acces
  * Generate an SSH key-pair locally and add the public key to the list of allowed SSH keys in the GitHub settings of your account.
- 2: Do the rest
  * Link the main branch of your local repository with the main branch of the GitHub repository

        git remote add origin git@github.com:<username>/<reponame>.git
        git fetch
        git branch --set-upstream-to=origin/main main

  * Do a `git pull` to pull the README and License files into your local repository.
  * Do a `git push` to push everything to GitHub.
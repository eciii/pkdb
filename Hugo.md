### Quick up and running instructions

I assume there is a _working directory_ (e.g `~/desk`) used for quick experiments like this.

- Download (e.g with `wget`) the latest version of Hugo from the Releases page of the [Hugo GitHub repository](https://github.com/gohugoio/hugo). Create the directory `~/desk/hugo` and extract the tar file there.
- Create the shell alias `alias hugo='~/desk/hugo/hugo'`
- Create a new Hugo site with the command `hugo new site ~/desk/quickstart`
- Download (e.g with `wget`) the latest version of the Docsy template from the Releases page of the [Docsy GitHub repository](https://github.com/google/docsy). Extract the tar file into `~/desk/quickstart/themes`.
- `cd` into `~/desk/quickstart` and create a post with the command `hugo new content posts/my-first-post.md`. Edit the file `content/posts/my-first-post.md` to add text to the post. Also change the `draft` property to `false`.
- Append the line `theme = 'docsy-x.y.z'`, where `x.y.z` is the downloaded version of Docsy, to the file `hugo.toml`. In that file also adjust the value of `baseURL` to some URL where the site will be served, e.g `http://localhost:8080`.
- Generate the site with the command `hugo -d ~/desk/public` and start a file server on that directory, e.g by using `python3 -m http.server`.
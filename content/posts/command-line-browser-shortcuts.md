---
title: "Command Line Browser Shortcuts - Git"
date: 2018-05-11
tags: ["command line", "git"]
draft: false
---
The other day I was making some maintenance changes across a bunch of different git repos, and for each change I need to open a Merge Request in GitLab. This meant that I needed to browse to each repo's page in GitLab, which is not too bad if the git hosting software you're using is speedy, however our instance of GitLab was... not.

After sitting watching the GitLab loading bar for seconds at a time, I thought "_there must be a better way to do this from the command line_". What I wanted was to be able to write a super-short command in my terminal, for the command to figure out which git repo I'm in, what the url of that repo's homepage is, and then open that url in my browser. Oh, and it should work across git hosting services too, since I have some personal projects on Github.

Enter the `og`:

```bash
function og() {
    origin_ssh_url=$(git config --get remote.origin.url)
    host=$(echo $origin_ssh_url | cut -d: -f1 | cut -d@ -f2)
    repo_name=$(echo $origin_ssh_url | cut -d: -f2 | cut -d. -f1)
    open "https://$host/$repo_name"
}
```

There's probably a better way to achieve the same result without the gratuitous use of `cut`, but the above was written in a flash of "_good grief why haven't I written something like this before?!_".

The idea is simple - extract the host & repo name and then use those to construct a url that we can pass to `open`. The great thing about this approach is it's independent of any particular git hosting service; I've personally tested it with an internally-hosted GitLab instance as well as the ubiquitous github.com.

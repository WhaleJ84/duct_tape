# duct\_tape

This repository contains files used to install `ansible` on a new machine and entirely configure it to my whims.

Currently it is only going to focus on `Ubuntu 20.04` but I will expand it further as needed in the future.

## Running the script

The following command will grab the contents of `bootstrap` from the web and run it locally - meaning that the repository doesn't need to be pulled down.

`curl https://raw.githubusercontent.com/WhaleJ84/duct_tape/main/bootstrap | sudo bash`

If you have pulled it locally, you can just run `./bootstrap` or `curl file:///PATH/TO/SCRIPT/bootstrap | sudo bash`

Either way, the commands will all do the same thing.

## Git hooks

As the `.git` directory is ignored by default, I will note the specific hooks I use locally.

- commit-msg
	- Check for any typos
```
#!/bin/sh

spelling_mistakes=$(grep -v "^ " "$1" | aspell list)

if [ "${#spelling_mistakes[@]}" -gt 0 ]; then
    printf "%s\n" "Spelling errors detected in commit message:" "$spelling_mistakes"
    exit 1
fi
```

- pre-commit
	- Lint and syntax check code.
```
#!/bin/sh

yamllint . || exit 1
ansible-playbook local.yml --syntax-check || exit 1
ansible-lint . || exit 1
```

- pre-push
	- I plan to use `molecule test` but am experiencing a [bug](https://stackoverflow.com/questions/35176548/authentication-or-permission-failure-did-not-have-permissions-on-the-remote-di://stackoverflow.com/questions/35176548/authentication-or-permission-failure-did-not-have-permissions-on-the-remote-dir) that no answers have fixed.
I am hoping Ansible 2.11 will fix this.

## Inspirations

[Jeff Geerling](https://www.youtube.com/playlist?list=PL2_OBreMn7FqZkvMYt6ATmgC0KAGGJNAN)
[LearnLinuxTV](https://www.youtube.com/watch?v=gIDywsGBqf4)


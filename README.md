# Support Custom User Hooks in Husky

Demo GitHub repo illustrating how best to set up **custom** user hooks in Husky. "Custom" hooks here means hooks that are specific to a user's project and not automatically installed for all users.

## Creating a custom user hook

To support custom user hooks, we add them to a `.husky/custom/` directory and run them using a `custom-hook` script appended to each regular Husky hook. These user hooks run after the regular hooks.

### Example

Assume we have a `pre-commit` Husky hook like this:

```
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

echo "Lint ALL THE THINGS!!!"
```

We want to allow users to specify their own additional hooks, say also running the test suite. To do that we use `.husky/custom-hook` to optionally call the corresponding hook in `.husky/custom/`.

Then we can just add `&& .husky/custom-hook pre-commit` to the hook:

```
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

echo "Lint ALL THE THINGS!!!" && .husky/custom-hook pre-commit
```

Then we can define our user-specific hook `.husky/custom/pre-commit`:

```
$ cat <<EOF > .husky/custom/pre-commit
heredoc> #!/bin/sh
heredoc> echo 'my custom hook!'
heredoc> EOF
$ chmod u+x .husky/custom/pre-commit
```

When running `pre-commit`, it now runs the standard Husky hook along with the user's custom hook.

```
$ git add . && git commit -m "cool change"
Lint ALL THE THINGS!!!
Running custom hook for pre-commit...
my custom hook!
```

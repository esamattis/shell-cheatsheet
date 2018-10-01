
# (Personal) Shell Cheatsheet

Things I have to look up too often for the old dusty `/bin/sh` (not bash).

## Echo to stderr

```sh
>&2 echo "Warning message"
```

## Functions

```sh
my_func() {
    local arg1="$1"
    local my_local_var="foo"

    # error
    return 1
}

```

## Default values

```sh
arg="${0:-default}"

if [ "${myvar:-default}" = "something" ]; then
  # has something in myvar
fi
```

## Redirect output from within the script


Send stdout to a file

    exec > file

with stderr

    exec > file
    exec 2>&1

These will truncate the file. To append use

    exec >> file
    exec 2>&1

## Handle exit status manually with `set -e`

    ret=0
    command_might_fail || ret=$?
    

## File extension

```js
filename="foo.txt"
extension="${filename##*.}" # txt
name="${filename%.*}" # foo
```

## Multiple conditions

```js
foo=1
bar=0

if [ "$foo" = "1" ]; then
    echo "foo"
fi

if [ "$foo" = "1" -o "$bar" = "1" ]; then
    echo "foo or bar"
fi


if [ "$foo" = "1" -a "$bar" = "1" ]; then
    echo "foo and bar"
fi
```

## Parse CLI options


```sh
while true; do
    case "${1:-}" in
    -o|--option)
        shift
        option="$1"
        shift
       ;;
    -f|--boolean-flag)
        shift
        flag=true
       ;;
    -h|--help)
        help && exit 0
        ;;
    "")
        break
        ;;
    *)
        argument="$1"
        shift
        ;;
    esac
done
```

## Multiline strings

```sh
cat > config.json << WHATEV
{
    "defaultLanguage": "$lang"
}
WHATEV


curl -d @- -X POST -H 'Content-Type: application/json' http://localhost:8080/log << JSON
{
  "type": "wlan",
  "date": "1348134879",
  "wlaninterface": "wlan0",
  "event": "AP-STA-CONNECTED",
  "mac": "e4:d5:3d:testmac",
  "relay_timestamp": 1348134880
}
JSON

```

## Shortable date

    date +%Y-%m-%d:%H:%M:%S
    
# Others

## Git commit edit

https://stackoverflow.com/a/1186549/153718

You can use git rebase, for example, if you want to modify back to commit `bbc643cd`, run

    $ git rebase --interactive 'bbc643cd^'

In the default editor, modify `pick` to `edit` in the line whose commit you want to modify. Make your changes and then commit them with the same message you had before:

    $ git commit --all --amend --no-edit

to modify the commit, and after that

    $ git rebase --continue

to return back to the previous head commit.

**WARNING**: Note that this will change the SHA-1 of that commit **as well as all children** -- in other words, this rewrites the history from that point forward. [You can break repos doing this](https://stackoverflow.com/a/3926832/1269037) if you push using the command `git push --force`

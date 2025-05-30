# ren - Rename files using wildcards or regex patterns

A command-line utility that renames files with a simple and intuitive
syntax, using wildcards or regex patterns.

It is a lightweight, safe, and portable Bash script for renaming files
in bulk using **wildcard patterns** or **regular expressions**.

Supports look-ahead conflicts detection, and dry-run previews for
safely operation.

## Features

### 1. Pattern matching

- Supports shell-style **wildcards**:
  - `*` matches any sequence of characters
  - `?` matches a single character
- Supports **Perl-style regex**
- Multiple wildcards can be used and referenced as `#1`, `#2`, ...
- Supports filenames with:
  - Spaces
  - Unicode characters

### 2. Positional substitution

- Use `#1`, `#2`, etc., in the target name to insert matched text from the input pattern
- `#0` represents the full match (optional)

### 3. Dry-run mode

- Safe for testing batch operations
- The script only **prints `mv` commands**

### 4. Portability

- No external dependencies
- Works on any system with Bash ≥ 4

### 5. Safety
- Does **not** support filenames with embedded newlines
- Scope is limited to files in current directory.
- If multiple files would be renamed to the same target name, the script **aborts with an error**
- Prevents accidental overwrites

## Limitations

| Limitation                    | Notes
|-------------------------------|--------------------------------------------------------
| **Files only**                | Only regular files are renamed — directories are skipped
| **No recursion**              | Files in the current directory only
| **No undo**                   | Rollback is not supported, but dry-run mode is available for testing
| **No built-in overwrite flag**| Script prevents overwrites — `force` mode not supported
| **No mkdir or path editing**  | All renames occur within current directory — paths can't be changed

## Install

```
cd /tmp
git clone https://github.com/rogeriooferraz/ren.git
sudo cp ren/ren /usr/local/bin/
sudo chmod +x /usr/local/bin/ren
rm -rf /tmp/ren
cd -
```

## Uninstall

```
sudo rm /usr/local/bin/ren
```

## Usage
```
ren [OPTIONS] pattern replacement

ARGUMENTS:
  pattern        : Existing filename pattern
                   Accepts wildcards by default,
                   or regular expressions (see OPTIONS)

  replacement    : New filename pattern
                   Accepts Positional substitution,
                   like `#1`, `#2`, etc. (see EXAMPLES)
OPTIONS:
  -d, --dry-run  : Enable dry run mode (no changes are made)
  -D, --debug    : Enable debug output
  -h, --help     : Display usage information
  -V, --version  : Show current version
```

## Examples

### 1. Rename using wildcards

Remove blank spaces and improve readability:

```
$ ren 'Screenshot from * ??-??-??.png' 'Screenshot_#1_(#2#3:#4#5:#6#7).png'
```

Renames:
```
Screenshot from 2025-05-10 22-52-47.png → Screenshot_2025-05-10_22:52:47.png
```

### 2. Rename using regex

```
$ ren -E '^img_([0-9]+)\.jpg$' 'image-#1.jpg'
```

Renames:
```
img_001.jpg → image-001.jpg
img_999.jpg → image-999.jpg
```

### 3. Add prefix and suffix

```
$ ren '*.txt' 'new-#1-ToDo.txt'
```

Renames:
```
file.txt → new-file-ToDo.txt
```

## Tips

- Test first: use **dry-run** mode to simulate actions without actually renaming.
- Avoid quoting glob patterns unless needed — Bash expands them before passing to the script.
- For advanced capture, use regex mode, and set capture groups like `([a-z]+)_([0-9]+)`

## License

This is free and open source software, under the terms of MIT License.<br>
There is NO WARRANTY, to the extent permitted by law.

**Project page**: https://github.com/rogeriooferraz/ren

## Author

Rogerio O. Ferraz (<rogerio.o.ferraz@gmail.com>)

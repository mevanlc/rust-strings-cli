# rust-strings-cli
**CLI for iddohau/rust-strings**

This project defines a CLI (`pyproject.toml`: `[project.scripts]`) interface for [iddohau/rust-strings](https://github.com/iddohau/rust-strings).
Arguments are parsed with Python's argparse.
Files are acquired via Python's glob.glob

```
Usage: rust-strings [options] [globs...]

The output format is line-oriented: one search result per line:
{path}\t{offset}\t{string}

  -a, --abs-path           Report absolute paths. Without this flag, paths are relativized to the CWD (iff they are children of the CWD).
  -e, --encodings <encs>   Comma separated list of encodings to consider while searching for strings (default: ascii,utf8)
  -m, --min <length>       Strings below this length will not be found (default: 3)
  -o, --output <file>      Write report to <file>. Output is written as UTF-8. Without this flag, the report is written to stdout.
  -f, --filter <regex>     Only strings matching this regex will be reported
  -j, --json                  Write search results as JSON instead of plain text. The root value is an object, where:
                               - Keys are file paths (strings).
                               - Each key maps to an array of results.
                               - Each result is a 2-element array: [match_string, offset].

```

## 

## Example of the rust_strings Python API:

```python
# pip install rust-strings

import rust_strings

# Get all ascii strings from file with minimun length of string
rust_strings.strings(file_path="/bin/ls", min_length=3)
# [('ELF', 1),
#  ('/lib64/ld-linux-x86-64.so.2', 680),
#  ('GNU', 720),
#  ('.<O', 725),
#  ('GNU', 756),
# ...]

# You can also set buffer size when reading from file (default is 1mb)
rust_strings.strings(file_path="/bin/ls", min_length=5, buffer_size=1024)

# You can set encoding if you need (default is 'ascii', options are 'utf-16le', 'utf-16be')
rust_strings.strings(file_path=r"C:\Windows\notepad.exe", min_length=5, encodings=["utf-16le"])

# You can set multiple encoding
rust_strings.strings(file_path=r"C:\Windows\notepad.exe", min_length=5, encodings=["ascii", "utf-16le"])

# You can also pass bytes instead of file_path
rust_strings.strings(bytes=b"test\x00\x00", min_length=4, encodings=["ascii"])
# [("test", 0)]

# You can also dump to json file
rust_strings.dump_strings("strings.json", bytes=b"test\x00\x00", min_length=4, encodings=["ascii"])
# `strings.json` content:
# [["test", 0]]
```

# Black `--extend-exclude` bug

## 💥 Quickstart
```
pip install black==22.6.0
```

## To reproduce the autoformat failure, run

```
black --check --verbose .
```

The following error occurs

```
Usage: black [OPTIONS] SRC ...
Try 'black -h' for help.

Error: Invalid value for '--extend-exclude': Not a valid regular expression: nothing to repeat at position 179 (line 5, column 5)
```

## To fix the error,

In `pyproject.toml`'s `--extend-exclude` replace ~~*_pb2.py~~ with .*_pb2.py

```
(
  ^/foo.py    # exclude a file named foo.py in the root of the project
  | .*_pb2.py  # exclude autogenerated Protocol Buffer files anywhere in the project
)
```

Run

```
black --check --verbose .
```

Now it's all fixed

```
...
Using configuration from project root.
subdir/should_be_skipped_pb2.py ignored: matches the --extend-exclude regular expression
should_be_skipped_pb2.py ignored: matches the --extend-exclude regular expression
would reformat should_be_reformatted.py
would reformat subdir/should_be_reformatted.py

Oh no! 💥 💔 💥
2 files would be reformatted.
```
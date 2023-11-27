If you don't already, it's a good idea to get into the habit of invoking python with the `-m` flag, e.g.

```shell
$ python -m pip install -r requirements.txt
```

This makes python execute `pip` as a script. More info on this below.

If executing this in an environment with multiple versions of python on the path, the system will execute the command using the first version of `python` it finds.

When starting a new project where you want to use Python 3.10, you create a virtual environment and activate it.

```shell
python3.10 -m venv .venv
```
```shell
source .venv/bin/activate
```

The virtual environment is now active using Python 3.10, and using `python` in a shell starts the 3.10 interpreter. But if you type

```shell
pip install -r requirements.txt
```

The version of `pip` in use here is ambiguous. To avoid this ambiguity, use `python -m`

```shell
python -m pip install -r requirements.txt
```

The `-m` flag ensures that you are using `pip` linked to the active python version.

### Executing a module as a script (with `-m`)

Python modules are typically written to be imported so that the available classes, functions and variables can be used by other modules/scripts. But if you were writing a utility in python and wanted to run it, you'd write a script which would need to include the block,

```python
if __name__ == "__main__":
```

Thinking about django, you'll find this in `manage.py`:

```python
#!/usr/bin/env python
import os
import sys

if __name__ == "__main__":
    os.environ.setdefault("DJANGO_SETTINGS_MODULE", "project.settings")

    from django.core.management import execute_from_command_line

    execute_from_command_line(sys.argv)
```

The two ways to execute this script would be `python manage.py` or `python -m manage`. This would cause the code in the `if` block to run.

Thinking again of the utility function, you may write something that can be executed by itself, or imported by your django project to be ran by the application. So consider the module `utils.py`:

```python
#!/usr/bin/env python

def do_something():

    print("Doing something awesome with python")


def main():
    do_something()


if __name__ == "__main__":
    main()
```

This script could be invoked by `python -m utils` to run `do_something` via `main`, or you could `from utils import do_something` and execute `do_something()` in another python module. Because `if __name__ == "__main__":` is not executed by importing the module, only executing the module as a script.

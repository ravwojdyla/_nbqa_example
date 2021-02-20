Reproduce:

```bash
➜  mypy_example git:(master) pre-commit run nbqa-mypy --all-files
nbqa-mypy................................................................Failed
- hook id: nbqa-mypy
- exit code: 1

pipelines/ex.ipynb:cell_2:1: error: Cannot find implementation or library stub for module named 'pandas'  [import]
pipelines/ex.ipynb:cell_2:1: note: See https://mypy.readthedocs.io/en/latest/running_mypy.html#missing-imports
pipelines/ex.ipynb:cell_3:1: error: Name 'foo2' is not defined  [name-defined]
Found 2 errors in 1 file (checked 1 source file)

➜  mypy_example git:(master) nbqa mypy .
pipelines/ex.ipynb:cell_3:1: error: Name 'foo2' is not defined
Found 1 error in 1 file (checked 2 source files)

➜  mypy_example git:(master) pre-commit run mypy --all-files
mypy.....................................................................Failed
- hook id: mypy
- exit code: 1

pipelines/std.py:5: error: Name 'foo2' is not defined  [name-defined]
Found 1 error in 1 file (checked 4 source files)

(tr-dev) ➜  mypy_example git:(master) ✗ mypy .
pipelines/std.py:5: error: Name 'foo2' is not defined
Found 1 error in 1 file (checked 4 source files)
```

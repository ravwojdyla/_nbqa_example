Reproduce:

```bash
(tr-dev) ➜  _nbqa_example git:(master) pre-commit run -a
black....................................................................Passed
nbqa-mypy................................................................Failed
- hook id: nbqa-mypy
- exit code: 1

pipelines/ex.py:2: error:
Module 'my_utils' has no attribute 'foo'  [attr-defined]
    from my_utils import foo
    ^
my_utils/baz/notebook/ex.ipynb:cell_1:1: error:
Module 'my_utils' has no attribute 'foo'  [attr-defined]
    from my_utils import foo
    ^
Found 2 errors in 2 files (checked 2 source files)

mypy.....................................................................Passed
```

Notice:
 * plain mypy passes
 * nbqa-mypy fails in `pipelines/ex.py` (!)
 * nbqa-mypy fails in `my_utils/baz/notebook/ex.ipynb`
 * nbqa-mypy doesn't fail in `pipelines/ex.ipynb`


Now:

```bash
(tr-dev) ➜  _nbqa_example git:(master) g rm my_utils/baz/notebook/ex.ipynb
rm 'my_utils/baz/notebook/ex.ipynb'
(tr-dev) ➜  _nbqa_example git:(master) ✗ g st
On branch master
Your branch is ahead of 'origin/master' by 3 commits.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        deleted:    my_utils/baz/notebook/ex.ipynb

(tr-dev) ➜  _nbqa_example git:(master) ✗ pre-commit run -a
black....................................................................Passed
nbqa-mypy................................................................Passed
mypy.....................................................................Passed
```

You can also fix it by applying this patch:

```
diff --git a/my_utils/__init__.py b/my_utils/__init__.py
index 863befe..e69de29 100644
--- a/my_utils/__init__.py
+++ b/my_utils/__init__.py
@@ -1,3 +0,0 @@
-from my_utils.baz.foo import foo
-
-__all__ = ["foo"]
diff --git a/my_utils/baz/notebook/ex.ipynb b/my_utils/baz/notebook/ex.ipynb
index a3d6519..6bdfbe5 100644
--- a/my_utils/baz/notebook/ex.ipynb
+++ b/my_utils/baz/notebook/ex.ipynb
@@ -6,7 +6,7 @@
    "metadata": {},
    "outputs": [],
    "source": [
-    "from my_utils import foo"
+    "from my_utils.baz.foo import foo"
    ]
   },
   {
diff --git a/pipelines/ex.ipynb b/pipelines/ex.ipynb
index a3d6519..6bdfbe5 100644
--- a/pipelines/ex.ipynb
+++ b/pipelines/ex.ipynb
@@ -6,7 +6,7 @@
    "metadata": {},
    "outputs": [],
    "source": [
-    "from my_utils import foo"
+    "from my_utils.baz.foo import foo"
    ]
   },
   {
diff --git a/pipelines/std.py b/pipelines/std.py
index cb97531..5199db5 100644
--- a/pipelines/std.py
+++ b/pipelines/std.py
@@ -1,4 +1,4 @@
-from my_utils import foo
+from my_utils.baz.foo import foo
 import pandas
 
 foo()
```

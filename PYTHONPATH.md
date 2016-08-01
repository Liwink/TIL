## PYTHONPATH

1. 之前运行 OpenPlay 项目时，都需要使用 `export` 命令指定 `PYTHONPATH`  到 `pylibs` 的目录。
   目的是，在项目中引入 pylibs 时，不需要考虑相对路径，直接使用 `import pylibs`。

2. 考虑在项目运行时，自动将 pylibs 目录添加到系统路径中，这样就不需要每次都手动指定。命令类似如下，项目启动时载入配置：

   ```python
   import os
   import sys
   PROJECT_PATH = os.path.realpath(os.path.dirname(__file__) + "/../..")
   sys.path.append(PROJECT_PATH) if PROJECT_PATH not in sys.path else None
   ```

3. 但上面方式不好，一是写法很不优雅，二是用命令行启动时，还需要先载入配置才能引入 pylibs，还是很麻烦。

4. 简单粗暴的办法是，在 .zshre 中加入 `exprot PYTHONPATH=/Users/Liwink/projects/openplay_backend/`，解决了所有问题

5. python 中常规引入，多用相对路径，就可以避免上面的问题，在就是第三方库的引用，原理上也是将第三方库的路径加入配置环境中，这里 pylibs 就类似于第三方库，可以考虑这样引入。



#### how python finds the modules

- some functions and classes(str, len, Exception, etc.) in Python are available in the global scope(called **builtin** scope in Python)

- others need to be imported by an **import** statement, but how Python know the *location of these modules*:

  - some are set automatically when you install the Python virtual machine (the packages path is available in **sys.path**)
    `sys.path.insert(0, 'path/to/my/packages')`
  - `PYTHONPATH` is a environment variable to augment the default package search paths
    `export PYTHONPATH=/path/to/some/directory:$PYTHONPATH`

  more elegant way?

#### the difference between a module and a package

A package is just a module or a collection of modules comes compressed inside a tarball, which contains:

1. information on dependencies
2. instructions to copy the files to the standard package search location
3. compile instructions



##### reference 

[PYTHON ECOSYSTEM AN INTRODUCTION](http://mirnazim.org/writings/python-ecosystem-introduction/)
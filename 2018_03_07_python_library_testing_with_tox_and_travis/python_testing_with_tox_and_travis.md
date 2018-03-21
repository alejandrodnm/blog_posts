# Testing against multiple python versions with tox

At work, while planning a refactor of one of our applications, we saw that some
of the modules we had already written in a different project could be reused
for what we were planning. I quickly thought, well, I can just move this to
it's one package, add it to both projects as a dependency and the gods of DRY
will be please with me.

Later that day, out of the blue, a realization came to me, the project to
refactor is the last one we have that runs on Python 2.7 (I know #SHAME),
while all the other use 3.5. Trying to upgrade the python version is
not an option, hey we still have two years according to the
[pythonclock](https://pythonclock.org/), so this was the perfect opportunity to
try tox.

From the [official docs](https://tox.readthedocs.io/en/latest/):

> tox is a generic virtualenv management and test command line tool you can use for:
>
> - checking your package installs correctly with different Python versions and interpreters
> - running your tests in each of the environments, configuring your test tool of choice
> - acting as a frontend to Continuous Integration servers, greatly reducing boilerplate and merging CI and shell-based testing.

To start using tox we need to do three things:

1. Install tox with `pip install tox`.
2. Create a setup.py for our package.
3. Create a tox.ini file at the same directoty level than the setup.py file.

The first step is just executing the `pip install tox` command, you can install
it in it's own virtualenv with the rest of the package requirements.

The next steo is to create the setup.py file, ours looks somethings like this:

```
from setuptools import find_packages, setup

setup(
    name='my-tox-tested-package',
    version='0.0.1',
    packages=find_packages(exclude=['tests']),  # Include all the python modules except `tests`.
    description='My custom package tested with tox',
    long_description='A long description of my custom package tested with tox',
    install_requires=[
        'Django>=1.11.0',
        'djangorestframework>=3.6.0',
        # Additional requirements, or parse the requirements file and add it here
    ],
    classifiers=[
        'Programming Language :: Python',
    ],
    entry_points={
        'pytest11': [
            'tox_tested_package = tox_tested_package.fixtures'
        ]
    },
)
```

This is a really simple setup.py file, it defines the package metadata and
it's requirements. This file will let us install our package as a dependency
in other projects, wich we can do locally with `pip install -e <path_to_package>`,
or uploading it to github or [pypi](https://pypi.python.org/pypi) and adding it
to our requirements file. You can find more about packagin in the
[Python Packagin User Guide](https://packaging.python.org/) and in the
[Setuptools documentation](https://setuptools.readthedocs.io/en/latest/index.html).

You may hava noticed the `entry_point` argument passed to the `setup` function,
the entry `pytest11` is used to make
[pytest plugins installable by others](https://docs.pytest.org/en/latest/writing_plugins.html#making-your-plugin-installable-by-others),
in our case is used to expose custom fixtures defined in our package, so the
projects that install our package can reuse them in their tests.

Finally we need to create the tox.ini file, again here's a sample of the one we
use:

```
# tox (https://tox.readthedocs.io/) is a tool for running tests
# in multiple virtualenvs. This configuration file will run the
# test suite on all supported python versions. To use it, "pip install tox"
# and then run "tox" from this directory.

[tox]
envlist = py27, py35

[testenv]
commands =
  pytest {posargs: tests}
  isort --check-only --diff --recursive --skip .tox --skip migrations
  flake8
deps =
  -rrequirements.txt
```

This is a really simple example, the envlist in the tox section specifies that
we want to run the commands specified in the testenv section against two
versions of python, in this case our targets are 2.7 and 3.5. Tox will work
by creating a separate virtualenv for each version and installing our package
in both of them.

If we want to test only one environment at a time we can pass the `-e` flag to
tox, for example, to test only python 3.5 we execute:

```
$ tox -e py35
```

The command entry in the testenv section specifies what tox will execute for
testing our code. The following will be called for each environment:

- pytest for unittest.
- isort for checking imports order.
- flake8 for linting.

You may have notice that pytest is acompany by `{posargs: tests}`,


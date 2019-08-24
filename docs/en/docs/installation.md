# Installation

The easiest way to install pandas is to install it
as part of the [Anaconda](http://docs.continuum.io/anaconda/) distribution, a
cross platform distribution for data analysis and scientific computing.
This is the recommended installation method for most users.

Instructions for installing from source,
[PyPI](https://pypi.org/project/pandas), [ActivePython](https://www.activestate.com/activepython/downloads), various Linux distributions, or a
[development version](http://github.com/pandas-dev/pandas) are also provided.

## Plan for dropping Python 2.7

The Python core team plans to stop supporting Python 2.7 on January 1st, 2020.
In line with [NumPy’s plans](https://github.com/numpy/numpy/blob/master/doc/neps/nep-0014-dropping-python2.7-proposal.rst#plan-for-dropping-python-27-support), all pandas releases through December 31, 2018
will support Python 2.

The 0.24.x feature release will be the last release to
support Python 2. The released package will continue to be available on
PyPI and through conda.

If there are people interested in continued support for Python 2.7 past December
31, 2018 (either backporting bug fixes or funding) please reach out to the
maintainers on the issue tracker.

For more information, see the [Python 3 statement](http://python3statement.org/) and the [Porting to Python 3 guide](https://docs.python.org/3/howto/pyporting.html).

## Python version support

Officially Python 2.7, 3.5, 3.6, and 3.7.

## Installing pandas

### Installing with Anaconda

Installing pandas and the rest of the [NumPy](http://www.numpy.org/) and
[SciPy](http://www.scipy.org/) stack can be a little
difficult for inexperienced users.

The simplest way to install not only pandas, but Python and the most popular
packages that make up the [SciPy](http://www.scipy.org/) stack
([IPython](http://ipython.org/), [NumPy](http://www.numpy.org/),
[Matplotlib](http://matplotlib.org/), …) is with
[Anaconda](http://docs.continuum.io/anaconda/), a cross-platform
(Linux, Mac OS X, Windows) Python distribution for data analytics and
scientific computing.

After running the installer, the user will have access to pandas and the
rest of the [SciPy](http://www.scipy.org/) stack without needing to install
anything else, and without needing to wait for any software to be compiled.

Installation instructions for [Anaconda](http://docs.continuum.io/anaconda/)
[can be found here](http://docs.continuum.io/anaconda/install.html).

A full list of the packages available as part of the
[Anaconda](http://docs.continuum.io/anaconda/) distribution
[can be found here](http://docs.continuum.io/anaconda/pkg-docs.html).

Another advantage to installing Anaconda is that you don’t need
admin rights to install it. Anaconda can install in the user’s home directory,
which makes it trivial to delete Anaconda if you decide (just delete
that folder).

### Installing with Miniconda

The previous section outlined how to get pandas installed as part of the
[Anaconda](http://docs.continuum.io/anaconda/) distribution.
However this approach means you will install well over one hundred packages
and involves downloading the installer which is a few hundred megabytes in size.

If you want to have more control on which packages, or have a limited internet
bandwidth, then installing pandas with
[Miniconda](http://conda.pydata.org/miniconda.html) may be a better solution.

[Conda](http://conda.pydata.org/docs/) is the package manager that the
[Anaconda](http://docs.continuum.io/anaconda/) distribution is built upon.
It is a package manager that is both cross-platform and language agnostic
(it can play a similar role to a pip and virtualenv combination).

[Miniconda](http://conda.pydata.org/miniconda.html) allows you to create a
minimal self contained Python installation, and then use the
[Conda](http://conda.pydata.org/docs/) command to install additional packages.

First you will need [Conda](http://conda.pydata.org/docs/) to be installed and
downloading and running the [Miniconda](http://conda.pydata.org/miniconda.html)
will do this for you. The installer
[can be found here](http://conda.pydata.org/miniconda.html)

The next step is to create a new conda environment. A conda environment is like a
virtualenv that allows you to specify a specific version of Python and set of libraries.
Run the following commands from a terminal window:

``` bash
$ conda create -n name_of_my_env python
```

This will create a minimal environment with only Python installed in it.
To put your self inside this environment run:

``` bash
$ source activate name_of_my_env
```

On Windows the command is:

``` bash
$ activate name_of_my_env
```

The final step required is to install pandas. This can be done with the
following command:

``` bash
$ conda install pandas
```

To install a specific pandas version:

``` bash
$ conda install pandas=0.20.3
```

To install other packages, IPython for example:

``` bash
$ conda install ipython
```

To install the full [Anaconda](http://docs.continuum.io/anaconda/)
distribution:

``` bash
$ conda install anaconda
```

If you need packages that are available to pip but not conda, then
install pip, and then use pip to install those packages:

``` bash
$ conda install pip
$ pip install django
```

### Installing from PyPI

pandas can be installed via pip from
[PyPI](https://pypi.org/project/pandas).

``` bash
$ pip install pandas
```

### Installing with ActivePython

Installation instructions for
[ActivePython](https://www.activestate.com/activepython) can be found
[here](https://www.activestate.com/activepython/downloads). Versions
2.7 and 3.5 include pandas.

### Installing using your Linux distribution’s package manager.

The commands in this table will install pandas for Python 3 from your distribution.
To install pandas for Python 2, you may need to use the ``python-pandas`` package.

Distribution | Status | Download / Repository Link | Install method
---|---|---|---
Debian | stable | [official Debian repository](http://packages.debian.org/search?keywords=pandas&searchon=names&suite=all&section=all) | sudo apt-get install python3-pandas
Debian & Ubuntu | unstable (latest packages) | [NeuroDebian](http://neuro.debian.net/index.html#how-to-use-this-repository) | sudo apt-get install python3-pandas
Ubuntu | stable | [official Ubuntu repository](http://packages.ubuntu.com/search?keywords=pandas&searchon=names&suite=all&section=all) | sudo apt-get install python3-pandas
OpenSuse | stable | [OpenSuse Repository](http://software.opensuse.org/package/python-pandas?search_term=pandas) | zypper in python3-pandas
Fedora | stable | [official Fedora repository](https://admin.fedoraproject.org/pkgdb/package/rpms/python-pandas/) | dnf install python3-pandas
Centos/RHEL | stable | [EPEL repository](https://admin.fedoraproject.org/pkgdb/package/rpms/python-pandas/) | yum install python3-pandas

**However**, the packages in the linux package managers are often a few versions behind, so
to get the newest version of pandas, it’s recommended to install using the ``pip`` or ``conda``
methods described above.

### Installing from source

See the [contributing guide](development/contributing.html#contributing) for complete instructions on building from the git source tree. Further, see [creating a development environment](development/contributing.html#contributing-dev-env) if you wish to create a *pandas* development environment.

## Running the test suite

pandas is equipped with an exhaustive set of unit tests, covering about 97% of
the code base as of this writing. To run it on your machine to verify that
everything is working (and that you have all of the dependencies, soft and hard,
installed), make sure you have [pytest](http://docs.pytest.org/en/latest/) >= 4.0.2 and [Hypothesis](https://hypothesis.readthedocs.io/) >= 3.58, then run:

``` python
>>> pd.test()
running: pytest --skip-slow --skip-network C:\Users\TP\Anaconda3\envs\py36\lib\site-packages\pandas
============================= test session starts =============================
platform win32 -- Python 3.6.2, pytest-3.6.0, py-1.4.34, pluggy-0.4.0
rootdir: C:\Users\TP\Documents\Python\pandasdev\pandas, inifile: setup.cfg
collected 12145 items / 3 skipped

..................................................................S......
........S................................................................
.........................................................................

==================== 12130 passed, 12 skipped in 368.339 seconds =====================
```

## Dependencies

Package | Minimum supported version
---|---
[setuptools](https://setuptools.readthedocs.io/en/latest/) | 24.2.0
[NumPy](http://www.numpy.org) | 1.13.3
[python-dateutil](https://dateutil.readthedocs.io/en/stable/) | 2.6.1
[pytz](http://pytz.sourceforge.net/) | 2017.2

### Recommended dependencies

- [numexpr](https://github.com/pydata/numexpr): for accelerating certain numerical operations.
``numexpr`` uses multiple cores as well as smart chunking and caching to achieve large speedups.
If installed, must be Version 2.6.2 or higher.
- [bottleneck](https://github.com/kwgoodman/bottleneck): for accelerating certain types of ``nan``
evaluations. ``bottleneck`` uses specialized cython routines to achieve large speedups. If installed,
must be Version 1.2.1 or higher.

::: tip Note

You are highly encouraged to install these libraries, as they provide speed improvements, especially
when working with large data sets.

:::

### Optional dependencies

Pandas has many optional dependencies that are only used for specific methods.
For example, [``pandas.read_hdf()``](reference/api/pandas.read_hdf.html#pandas.read_hdf) requires the ``pytables`` package. If the
optional dependency is not installed, pandas will raise an ``ImportError`` when
the method requiring that dependency is called.

Dependency | Minimum Version | Notes
---|---|---
BeautifulSoup4 | 4.6.0 | HTML parser for read_html (see [[[note](#optional-html)](#optional-html)](#optional-html))
Jinja2 |   | Conditional formatting with DataFrame.style
PyQt4 |   | Clipboard I/O
PyQt5 |   | Clipboard I/O
PyTables | 3.4.2 | HDF5-based reading / writing
SQLAlchemy | 1.1.4 | SQL support for databases other than sqlite
SciPy | 0.19.0 | Miscellaneous statistical functions
XLsxWriter | 0.9.8 | Excel writing
blosc |   | Compression for msgpack
fastparquet | 0.2.1 | Parquet reading / writing
gcsfs | 0.2.2 | Google Cloud Storage access
html5lib |   | HTML parser for read_html (see note)
lxml | 3.8.0 | HTML parser for read_html (see note)
matplotlib | 2.2.2 | Visualization
openpyxl | 2.4.8 | Reading / writing for xlsx files
pandas-gbq | 0.8.0 | Google Big Query access
psycopg2 |   | PostgreSQL engine for sqlalchemy
pyarrow | 0.9.0 | Parquet and feather reading / writing
pymysql | 0.7.11 | MySQL engine for sqlalchemy
pyreadstat |   | SPSS files (.sav) reading
pytables | 3.4.2 | HDF5 reading / writing
qtpy |   | Clipboard I/O
s3fs | 0.0.8 | Amazon S3 access
xarray | 0.8.2 | pandas-like API for N-dimensional data
xclip |   | Clipboard I/O on linux
xlrd | 1.1.0 | Excel reading
xlwt | 1.2.0 | Excel writing
xsel |   | Clipboard I/O on linux
zlib |   | Compression for msgpack

#### Optional dependencies for parsing HTML

One of the following combinations of libraries is needed to use the
top-level [``read_html()``](reference/api/pandas.read_html.html#pandas.read_html) function:

*Changed in version 0.23.0.* 

- [BeautifulSoup4](http://www.crummy.com/software/BeautifulSoup) and [html5lib](https://github.com/html5lib/html5lib-python)
- [BeautifulSoup4](http://www.crummy.com/software/BeautifulSoup) and [lxml](http://lxml.de)
- [BeautifulSoup4](http://www.crummy.com/software/BeautifulSoup) and [html5lib](https://github.com/html5lib/html5lib-python) and [lxml](http://lxml.de)
- Only [lxml](http://lxml.de), although see [HTML Table Parsing](user_guide/io.html#io-html-gotchas)
for reasons as to why you should probably **not** take this approach.

::: danger Warning

- if you install [BeautifulSoup4](http://www.crummy.com/software/BeautifulSoup) you must install either
[lxml](http://lxml.de) or [html5lib](https://github.com/html5lib/html5lib-python) or both.
[``read_html()``](reference/api/pandas.read_html.html#pandas.read_html) will **not** work with *only*
[BeautifulSoup4](http://www.crummy.com/software/BeautifulSoup) installed.
- You are highly encouraged to read [HTML Table Parsing gotchas](user_guide/io.html#io-html-gotchas).
It explains issues surrounding the installation and
usage of the above three libraries.

:::

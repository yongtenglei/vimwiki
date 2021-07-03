# python_install

1. Download Gzipped source tarball file

2. unzip

```bash
tar -zxvf your python .tgz file
```

3. cd your unzipped file

```bash
./configure --prefix=/your/python/path/what/you/like/python-3.x.x

# eg:
# ./configure --prefix=/usr/local/bin/python-3.7.5

make
sudo make install
```

4. create a soft link

```bash
# eg:
ln -s /usr/local/bin/python-3.7.5 /usr/local/bin/python3.7

# or:
ln -s /usr/local/bin/python-3.7.5 /usr/bin/python3.7
```
### Using virtualenv for python muti-version requirement

```bash
virtualenv -p /usr/bin/python3.7 my_env
```

### Active virtual environment

```bash
source my_env/bin/active
```

### Deactive virtual environment

```bash
deactive
```

# different between virtualenv and venv

```
virtualenv allow you to give a path to use different python version

just like
virtualenv -p /usr/bin/python3.7 my_env

=============================================================

venv using your mechine python version defultlly
```

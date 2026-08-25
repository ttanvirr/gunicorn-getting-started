This Gunicorn getting-started guide is from their [official documentation](https://gunicorn.org/quickstart/).

<!-- =================================================== -->

# Table of Contents <!-- omit in toc -->

- [1. Quickstart](#1-quickstart)
  - [1.1. Install](#11-install)
    - [1.1.1. Virtual Environment (Recommended)](#111-virtual-environment-recommended)
    - [1.1.2. Verify Installation](#112-verify-installation)
  - [1.2. Create an Application (Django)](#12-create-an-application-django)
  - [1.3. Run](#13-run)
  - [1.4. Add Workers](#14-add-workers)
  - [1.5. Bind to a Port](#15-bind-to-a-port)
  - [1.6. Configuration File](#16-configuration-file)
  - [1.7. Print and validate configuration](#17-print-and-validate-configuration)
- [2. Next Steps](#2-next-steps)

# 1. Quickstart

Get a Python web application running with Gunicorn in 5 minutes.

> [!NOTE]
> Gunicorn requires Python 3.12 or newer.

## 1.1. Install

### 1.1.1. Virtual Environment (Recommended)

Always install Gunicorn inside a virtual environment to isolate dependencies:

```bash
# Create virtual environment
python3 -m venv .venv

# Activate it
source .venv/bin/activate  # Linux/macOS
# or: .venv\Scripts\activate  # Windows

# Install gunicorn
pip install gunicorn
```

Add `.venv` to `.gitignore` file.

### 1.1.2. Verify Installation

Check the installed version:

```bash
gunicorn --version
```

Test with a simple WSGI application:

```bash
echo 'def app(e, s): s("200 OK", []); return [b"OK"]' > test_app.py
gunicorn test_app:app
# Visit gunicorn's default bind address http://127.0.0.1:8000
# and you will see response "OK"
```

## 1.2. Create an Application (Django)

Inside `.venv` install django:

`Terminal`

```bash
(.venv)$ pip install django
(.venv)$ pip freeze > requirements.txt
```

Create a django project

```bash
(.venv)$ django-admin startproject myproject .
```

Now, this django project already has a WSGI application at `myproject/wsgi.py`. No additional code is needed.

## 1.3. Run

`Terminal`

```bash
$ gunicorn myproject.wsgi
```

## 1.4. Add Workers

Use multiple workers to handle concurrent requests:

```bash
$ gunicorn myproject.wsgi --workers 4
```

A good starting point rule for number of workers: (`2 * CPU_CORES + 1`) workers. Thus if a server has 2 CPU cores, you could add `2*2+1=5` workers. But you have to consider many other things.

## 1.5. Bind to a Port

By default Gunicorn binds to `127.0.0.1:8000`. Change it with:

```bash
$ gunicorn myproject.wsgi --bind 0.0.0.0:8080
```

## 1.6. Configuration File

So far you've been giving Gunicorn options directly on the command line, like:

```bash
$ gunicorn myproject.wsgi --workers 4 --bind 127.0.0.1:8000
```

Create `gunicorn.conf.py` with following content for reusable settings:

```py
bind = "127.0.0.1:8000"
workers = 4
accesslog = "-"
```

Then simply run:

```bash
$ gunicorn myproject.wsgi
```

Gunicorn automatically loads `gunicorn.conf.py` from the current directory.

> [!NOTE]
> Options set on the command line override framework settings and values from the configuration file.

## 1.7. Print and validate configuration

Print the fully resolved configuration:

```bash
$ gunicorn --print-config myproject.wsgi
```

Validate configuration and exit:

```bash
$ gunicorn --check-config myproject.wsgi
```

This is also a quick way to confirm that your application can start.

# 2. Next Steps

- [Nginx basics](https://github.com/ttanvirr/nginx-basics)
- [Deploy/run Gunicorn behind Nginx proxy server](https://github.com/ttanvirr/gunicorn-deploy-behind-nginx)

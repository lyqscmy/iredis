<p align="center">
  <img width="100" height="100" src="https://raw.githubusercontent.com/laixintao/iredis/master/docs/assets/logo.png" />
</p>

<h3 align="center">Interactive Redis: A Cli for Redis with AutoCompletion and Syntax Highlighting.</h4>

<p align="center">
<a href="https://github.com/laixintao/iredis/actions"><img src="https://github.com/laixintao/iredis/workflows/Test/badge.svg" alt="Github Action"></a>
<a href="https://badge.fury.io/py/iredis"><img src="https://badge.fury.io/py/iredis.svg" alt="PyPI version"></a>
<img src="https://badgen.net/badge/python/3.6%20|%203.7%20|%203.8/" alt="Python version">
<a href="https://t.me/iredis_users"><img src="https://badgen.net/badge/icon/join?icon=telegram&amp;label=usergroup" alt="Chat on telegram"></a>
<a href="https://console.cloud.google.com/cloudshell/editor?cloudshell_git_repo=https://github.com/laixintao/iredis&amp;cloudshell_print=docs/cloudshell/run-in-docker.txt"><img src="https://badgen.net/badge/run/GoogleCloudShell/blue?icon=terminal" alt="Open in Cloud Shell"></a>
</p>

<p align="center">
  <img src="./docs/assets/demo.svg" alt="demo">
</p>

IRedis is a terminal client for redis with auto-completion and syntax highlighting. IRedis lets you type Redis commands smoothly, and displays results in a user-friendly format.

IRedis is an alternative for redis-cli. In most cases, IRedis behaves exactly the same as redis-cli. Besides, it is safer to use IRedis on production servers than redis-cli: IRedis will prevent accidentally running dangerous commands, like `KEYS *` (see [Redis docs / Latency generated by slow commands](https://redis.io/topics/latency#latency-generated-by-slow-commands)).


## Features

- Advanced code completion. If you run command `KEYS` then run `DEL`, IRedis will auto-complete your command based on `KEYS` result.
- Command validation. E.g. try `CLUSTER MEET IP PORT`, iredis will validate IP and PORT for you.
- Command highlighting, fully based on redis grammar. Any valid command in IRedis shell is a valid redis command.
- Human-friendly result display.
- `peek` command to check the key's type then automatically call `get`/`lrange`/`sscan`, etc, depending on types. You don't need to call the `type` command then type another command to get the value. `peek` will also display the key's length and memory usage.
- <kbd>Ctrl</kbd> + <kbd>C</kbd> to cancel the current typed command, this won't exit iredis, exactly like bash behaviour. Use <kbd>Ctrl</kbd> + <kbd>D</kbd> to send a EOF to exit iredis.
- Says "Goodbye!" to you when you exit!
- <kbd>Ctrl</kbd> + <kbd>R</kbd> to open **reverse-i-search** to search through your command history.
- Auto suggestions. (Like [fish shell](http://fishshell.com/).)
- Support `--encode=utf-8`, to decode Redis' bytes responses.
- Command hint on bottom, include command syntax, supported redis version, and time complexity.
- Offcial docs with built-in `HELP` command, try `HELP SET`!
- Written in pure Python, but IRedis was packaged into a single binary with
[PyOxidizer](https://github.com/indygreg/PyOxidizer), you can use cURL to
download and run, it just works, even you don't have a Python interpreter.
- For full features, please see: [iredis.io/show](https://www.iredis.io/show/)

## Install

Install via pip:

```
pip install iredis
```

[pipx](https://github.com/pipxproject/pipx) is recommended:

```
pipx install iredis
```

Or you can download the executable binary with cURL(or wget), untar, then run.
It is especially useful when you don't have a python interpreter(E.g. the
[official Redis docker image](https://hub.docker.com/_/redis/) which doesn't
have Python installed.):

```
wget  https://github.com/laixintao/iredis/releases/latest/download/iredis.tar.gz \
 && tar -xzf iredis.tar.gz \
 && ./iredis
```

(Check the [release page](https://github.com/laixintao/iredis/releases) if you
want to download an old version of IRedis.)

## Usage

Once you install IRedis, you will know how to use it. Just remember, IRedis
supports similar options like redis-cli, like `-h` for redis-server's host
and `-p` for port. 

```
$ iredis --help
```

### Configuration

IRedis supports config files. Command-line options will always take precedence
over config. Configuration resolution from highest to lowest precedence is:

- *Options from command line*
- `$PWD/.iredisrc`
- `~/.iredisrc` (this path can be changed with `iredis --iredisrc $YOUR_PATH`)
- `/etc/iredisrc`
- default config in IRedis package.

You can copy the *self-explained* default config here:

https://raw.githubusercontent.com/laixintao/iredis/master/iredis/data/iredisrc


And then make your own changes.

(If you are using an old verions of iredis, please use the config file below,
and change the version in url):

https://raw.githubusercontent.com/laixintao/iredis/v1.0.4/iredis/data/iredisrc

### Keys

IRedis support unix/readline-style REPL keyboard shortcuts, which means keys like
<kbd>Ctrl</kbd> + <kbd>F</kbd> to forward work.

Also:

- <kbd>Ctrl</kbd> + <kbd>F</kbd> (i.e. EOF) to exit; you can also use the `exit` command.
- <kbd>Ctrl</kbd> + <kbd>L</kbd> to clear screen; you can also use the `clear` command.
<<<<<<< HEAD
- <kbd>Ctrl</kbd> + <kbd>X</kbd> <kbd>Ctrl</kbd> + <kbd>E</kbd> to open an editor
=======
- <kbd>Ctrl</kbd> + <kbd>X</kbd> <kbd>Ctrl</kbd> + <kbd>X</kbd> to open an editor
>>>>>>> prevent running command immediately when exit editor editing.
to edit command, or <kbd>V</kbd> in vi-mode.

## Development

### Release Strategy

IRedis is built and released by CircleCI. Whenever a tag is pushed to the `master` branch, a new release is built and uploaded to pypi.org, it's very convenient.

Thus, we release as often as possible, so that users can always enjoy the new features and bugfixes quickly. Any bugfix or new feature will get at least a patch release, whereas big features will get a minor release.

### Setup Environment

IRedis favors [poetry](https://github.com/sdispater/poetry) as package management tool. To setup a develop enviroment on your computer:

First, install poetry (you can do it in a python's virtualenv):

```
pip install poetry
```

Then run (which is similar to `pip install -e .`):

```
poetry install
```

**Be careful running testcases locally, it may flush you db!!!**

### Development Logs

This is a command-line tool, so we don't write logs to stdout.

You can `tail -f ~/.iredis.log` to see logs, the log is pretty clear,
you can see what actually happens from log files.

### CI

We use [pexpect](https://pexpect.readthedocs.io/en/stable/) to test command-line
behavior. And since there are problems with CircleCI's tty, we run
pexpect-related tests on travis, and run unittest/black style check/flake8 check
on CircleCI.

For local development, just run `pytest`. If all tests pass locally, CI shall pass.

### Command Reference

There is a full Redis command list in [commands.csv](docs/commands.csv) file, downloaded by:

```
python scripts/download_redis_commands.py > data/commands.csv
```

`commands.csv` is here only for test if redis.io was updated, do not package it into release.

Current implemented commands: [command_syntax.csv](iredis/data/command_syntax.csv).

## Related Projects

- [redis-tui](https://github.com/mylxsw/redis-tui)

If you like iredis, you may also like other cli tools by [dbcli](https://www.dbcli.com/):

- [pgcli](https://www.pgcli.com) - Postgres Client with Auto-completion and Syntax Highlighting
- [mycli](https://www.mycli.net) - MySQL/MariaDB/Percona Client with Auto-completion and Syntax Highlighting
- [litecli](https://litecli.com) - SQLite Client with Auto-completion and Syntax Highlighting
- [mssql-cli](https://github.com/dbcli/mssql-cli) - Microsoft SQL Server Client with Auto-completion and Syntax Highlighting
- [athenacli](https://github.com/dbcli/athenacli) - AWS Athena Client with Auto-completion and Syntax Highlighting
- [vcli](https://github.com/dbcli/vcli) - VerticaDB client
- [iredis](https://github.com/laixintao/iredis/) -  Client for Redis with AutoCompletion and Syntax Highlighting

IRedis is build on the top of [prompt_toolkit](https://github.com/jonathanslenders/python-prompt-toolkit), a Python library (by [Jonathan Slenders](https://twitter.com/jonathan_s)) for building rich commandline applications.

[Heroku Buildpack API]: https://devcenter.heroku.com/articles/buildpack-api "Heroku Buildpack API"
[per docs]: https://devcenter.heroku.com/articles/buildpack-api


# Heroku Buildpack RC

## Description

Forward the buildpack API to a local app. 

## Installation

> See: [Heroku Buildpack CLI](https://devcenter.heroku.com/articles/buildpacks)

```bash
heroku buildpacks:add https://github.com/aubricus/heroku-buildpack-rc.git
```

## Usage

Create a directory named `herokurc` at the project root with the following structure.

```text
herokurc/
├── profile.d
│   └── {app-name}-env.sh
├── detect
├── compile
└── release
```

* `profile.d` is optional
* `profile.d/env.sh` is optional (`{app-name}` prefix is recommended [per docs], but `-env` suffix is arbitrary and not required)
* `detect` is required, must return an exit code of 1 or 0 [per docs]
* `compile` is required [per docs]
* `release` is optional [per docs]

## Utils

Scripts are sourced directly and so have access to a few utils sourced internally:

```bash
# indent_arrow
# Top-level output per heroku conventions
# ex. `echo "hello world" | indent_arrow`
#     => "------> hello world"
indent_arrow

# indent
# Sub-command output per heroku conventions
# ex. `echo "hello world" | indent`
#     => "        hello world"
indent
```

## Profile.d

> These files should only ever set ENV vars [per docs].

Heroku will source any `*.sh` file in a `.profile.d` directory before executing the dyno's command. 

This build pack white lists _only_ `*.sh` files [per docs] and will copy any `*.sh` file in the `herokurc/profile.d` directory to the Heroku `.profile.d/` directory. 

> __Notes:__ 
> 
> * Copy runs on every push and will always overwrite existing files.

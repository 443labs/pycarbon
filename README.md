# PyCarbon

A YAML-based configuration system for Python projects supporting tiered environments and files.

Why is it called `PyCarbon`? That's a horrible name...

* Naming things is the hardest task in programming. (opinion)
* There are a lot of modules already listed in PyPi, so I needed something.
* Carbon is the building block of life as we know. A good configuration system is the bulding block of any application.
* ...
* That's all I've got. Open to suggestions!

## Credits

This code based upon concepts and original source material from members of Karmic Labs, Inc. engineering group. 

Kudos and many thanks for allowing me to publish this derived work!

## Key Concepts**

* Environments: [`production`, `staging`, `development`]
* Files: [`config.yml`, `localhost.yml`, `secrets.yml`]
* Supports multiple YAML documents per file, namespaced by an `environment`. Each providing a baseline for the next.
* Supports multiple config files, with the opinion that some settings are not fit for source control and should be isolated.
* All settings are merged at runtime, allowing for easy querying.
* Setting keys may be simple or complex (i.e., `MY_SETTING` or `demo.category.foo`)

## Usage

```
from pycarbon import Configuration

# load files from the config directory in your project
config = Configuration(directory='./config')

# load a setting
print(config.get('demo.setting'))

# load a setting, throw an exception if it doesn't exist
print(config.get('demo.setting', exception=True))

# load a setting, with a default
print(config.get('demo.setting', default=42))

# use a complex key path to the setting (delimiters supported: comma, slash, colon)
print(config.get(`yaml.key.path.to.your.setting`))
print(config.get(`yaml:key:path:to:your:setting`))
print(config.get(`yaml/key/path/to/your/setting`))
```

## Environments

The following `environments` are supported by default, each overriding values from the previous.

* `production`
* `staging`
* `development`
* `testing`

Environments may be overriden by setting the `CONFIG_ENVS` environment variable, or by constructor injection.

```
# bash
export CONFIG_ENVS=`production`, `staging`, `development`
```

```
# python
config = Configuration(environments=['prod`, `stage`, `development`]
```

## Files

PyCarbon loads YAML from these files by default, each overriding values from the previous.

* `config.yml`: Default config file, commit this to your source control.
* `localhost.yml`: Developer config file, likely contains secrets for local development and is ignored by your VCS.
* `secrets.yml`: Top-most secrets file. This file should never be committed to VCS, and contains secrets needed for deployments (i.e., dev, staging, or production).

Files may be overridden by setting the `CONFIG_FILES` environment variable, or by constructor injection.

```
# bash
export CONFIG_FILES=`config.yml`, `localhost.yml`, `secrets.yml`
```

```
# python
config = Configuration(files=['config.yml`, `localhost.yml`, `secrets.yml`]
```

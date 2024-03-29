# evn-yaml

This is similar to what is done in Quarkus: https://quarkus.io/guides/config-reference#property-expressions

Allows for substitution of environment variables when loading a yaml file, while providing defaults, for e.g.:
```yaml
database:
  mongodb:
    connection_string: ${MONGODB_CONNECTION_STRING:localhost:27017}
    database: ${MONGODB_DATABASE_NAME:test}
```


## Usage


Create `config.yaml`:
```yaml
a:
  b: ${ENV_B:default_used_b}
  c: ${ENV_C:default_used_c}
  d: ${ENV_D}
```

Read file and use `EnvLoader`:
```python
import os

from env_yaml import EnvLoader
import yaml

os.environ['ENV_B'] = 'default_not_used_b'

with open('config.yaml', 'r') as f:
    file_contents = f.read()
    result = yaml.load(file_contents, EnvLoader)
print(result)
```

Results in:
```python
{'a': {'b': 'default_not_used_b', 'c': 'default_used_c', 'd': None}}
```

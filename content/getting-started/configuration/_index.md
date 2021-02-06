---
title: Configuration
date: "2020-01-31"
weight: 20
---

{{<toc>}}

## Stevedore configuration
When Stevedore runs, it tries to read its configuration firsly from environment variables and then from local configuration file, otherwise it uses the default values.

{{< hint warning >}}
The most precedence configuration is these which is loaded from `--config` flag's gived file.
{{< /hint>}}

### Configuration from environment variables
To set any configuration parameter from an environment variable, you must create an environment variable named as the parameter's name prefixed by `STEVEDORE_` uppercased.

For example, to define [tree_path]({{<ref "/getting-started/configuration/#tree_path">}}) value from an environment variable it must be named `STEVEDORE_TREE_PATH`.

### Configuration from local configuration file
By default, Stevedore looks for a configuration on the files listed below, and load the configuration from the first found file.
1. stevedore.yaml
2. ~/.config/stevedore/stevedore.yaml
3. ~/stevedore.yaml

You could also load the configuration from a custom location and it is done by `--config` flag when stevedore cli is launched. Loading configuration from a custom location has precedence over environment variables.

## Create configuration file
You can create a configuration file by yourself or you could use stevedore cli to do it.

Stevedore cli has the command `stevedore create configuration` that creates a configuration file. By default, the configuration file generated is placed on `./stevedore.yaml` but you can indicate your custom location setting the `--config` flag.

You could also use `stevedore init`, which is an alias for the command above.

{{< hint warning >}}
Check `stevedore create configuration` flags to define your configuration
{{< /hint>}}

### Stevedore credentials
Stevedore could store Docker registry's credentials to use in behalf you during the [building]({{<ref "/getting-started/concepts/#build">}}) or [promoting]({{<ref "/getting-started/concepts/#promote">}}) process.

Credentials are stored on its own folder which you could be set by [docker_registry_credentials_dir]({{<ref "/getting-started/configuration/#docker_registry_credentials_dir">}}) parameter. The default location for credentials is `~/.config/stevedore/credentials`.

In case you created your configuration using `stevedore create configuration`, your firts registry credentials would be also created, if you specified it.
You could create new credentials through `stevedore create credentials`.

{{< hint warning >}}
Check `stevedore create credentials` flags to define the registry host, username or password. Otherwise, an empty credential is created.
{{< /hint>}}

## Configuration parameters
Below you could find all the parameters that can be configure on Stevedore.

### **tree_path**
Images tree location path.
- Default value:
```yaml
    tree_path: stevedore.yaml
```

### **builder_path**
Builders location path.
- Default value:
```yaml
   builder_path: stevedore.yaml
```

### **log_path**
Log file location path.
- Default value:
```yaml 
    log_path: /dev/null
```

### **num_workers**
It defines the number of workers to build images which corresponds to the number of images that can be build concurrently.
- Default value:
```yaml 
    num_workers: 4
```

### **push_images**
On build, push images automatically after it finishes.
- Default value:
```yaml 
    push_images: true
```

### **build_on_cascade**
On build, start children images building once an image build is finished.
- Default value:
```yaml 
   build_on_cascade: false
```

### **docker_registry_credentials_dir**
Directory to store docker registry credentials.
- Default value:
```yaml 
    docker_registry_credentials_dir: ~/.config/stevedore/credentials
```

### **semantic_version_tags_enabled**
Generate extra tags when the main image tags is semver 2.0.0 compliance.
- Default value:
```yaml
    semantic_version_tags_enabled: false
```

### **semantic_version_tags_templates**
List of templates which define those extra tags to generate when 'semantic_version_tags_enabled' is enabled.
- Default value:
```yaml 
    semantic_version_tags_templates:  
    - {{ .Major }}.{{ .Minor }}.{{ .Patch }}
```

### **builders**
Define `global` builders. You could define `global` builders on its own file. Stevedore will look up for builders on the file set at [builder_path]({{<ref "/getting-started/configuration/#builder_path">}})
 For further information see the [reference guide]({{<ref "/reference-guide">}}).

### **images_tree**
Define images tree. You could define an images tree on its own file. Stevedore will look up for images tree on the file set at [tree_path]({{<ref "/getting-started/configuration/#tree_path">}})
 For further information see the [reference guide]({{<ref "/reference-guide">}}).

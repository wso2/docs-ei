# Managing Configurations across Environments

All the server configurations of the Micro Integrator are specified in a single
[TOML-based configuration file](../../references/config-catalog), which is the `deployment.toml` file (stored int he  `<MI_HOME>/conf` directory).

When you have multiple environments (Dev, QA, UAT, Prod), you need the flexibility of dynamically updating the configurations in this file. You can achieve this by defining your server configurations as environment variables or system properties in your server's configuration file. You can then separately inject configuration values for each environment.

In your `deployment.toml` file, you can define the configurations in one of three ways based on your preference.
Let's assume you want to set the server offset of your micro-integrator instance.

## Using System Properties

Use the following syntax in the `deployment.toml` file to specify a configuration as a system property:

```toml
parameter="$sys{systemPropertyName}"
```

**Example**:

In the following example, the value for the server offset parameter is specified by the `offset` system property.

```toml
[server]
offset = "$sys{offset}"
```

You can set the value for the `offset` system property during server startup. That is, when you execute the server startup command on your terminal, pass the system property value as shown below.

```bash
./micro-integrator.sh -Doffset=19
```

## Using Environment Variables

Use the following syntax in the `deployment.toml` file to specify a configuration as an environment variable:

```toml
parameter="$env{environmentVariableName}"
```

**Example**:

In the following example, the value for the server offset parameter is specified by the `offset` variable.

```toml
[server]
offset = "$env{offset}"
```

You can now set the environment variables you defined as follows before starting the server:

```bash
export offset=22
```

## Resolving Variables during Runtime

As opposed to defining the configuration parameter as `$sys{property}` or `$env{variable}`, if you use the following syntax in the deployment.toml file, you can pass configurations and resolve them during runtime. That is, you do not have to restart the server for the parameter values to become effective.

```toml
offset = "${VariableName}"
```

**Example**:

In the following example, the value for the server offset parameter is specified by the `offset` variable.

```toml
[server]
offset = "${offset}"
```

You can set a value for the `offset` variable as an environment variable or a system property and it will be resolved during runtime. If you have set the value as both a system property and an environmental variable, the system property will be effective.

*System property > Environment variable*

## Docker/Kubernetes environments

When you update product configurations for a container deployment (Docker or Kubernetes), you will update the `deployment.toml` file from your Docker Exporter project or Kubernetes Exporter project in WSO2 Integration Studio.

You can open the `deployment.toml` file from the project explorer and update the parameter values as [system properties](#system-properties) or [environment variables](#environment-variables).

<img src="../../assets/img/env-variable-support/k8s-project-deployment-file.img" width="500">

When you execute the `docker run` command to start the container, you can pass the system properties and environment variables.

!!! Tip
		Some of the server configurations that you have defined as a system variable or environment variable may be secrets that should be encrypted. Therefore, you must first encrypt these values [using the CLI tool](../../setup/security/encrypting_plain_text/#using-the-cli-tool) and then be sure to pass the encrypted secret when you execute the `docker run` command.

These values will be resolved dynamically during the runtime.

# Config file .credo.exs

Credo is configured via a file called `.credo.exs`. This file can live in your project's `config/` or root folder, both is fine.

You can use `mix credo gen.config` to generate a complete example configuration.

```elixir
# config/.credo.exs
%{
  configs: [
    %{
      name: "default",
      files: %{
        included: ["lib/", "src/", "web/", "apps/"],
        excluded: []
      },
      checks: [
        {Credo.Check.Consistency.TabsOrSpaces},

        # For some checks, like AliasUsage, you can only customize the priority
        # Priority values are: `low`, `normal`, `high`, `higher`
        {Credo.Check.Design.AliasUsage, priority: :low},

        # For others you can also set parameters
        {Credo.Check.Readability.MaxLineLength, priority: :low, max_length: 80},

        # You can also customize the exit_status of each check.
        # If you don't want TODO comments to cause `mix credo` to fail, just
        # set this value to 0 (zero).
        {Credo.Check.Design.TagTODO, exit_status: 2},

        # To deactivate a check:
        # Put `false` as second element:
        {Credo.Check.Design.TagFIXME, false},

        # ... several checks omitted for readability ...
      ]
    }
  ]
}
```

`mix credo --config-name <NAME_OF_CONFIG>` allows you to use a specific config (instead of `default`) inside a config file.

`mix credo --config-file <PATH_TO_CONFIG_FILE>` let's you use a specific config file.

## Transitive configuration files

Credo traverses the filesystem's folder structure upwards to find additional config files which it applies to the current project.

Consider the following directory structure:

```shell
/
  home/
    rrrene/
      projects/
        foo/
          .credo.exs
        bar/
      .credo.exs
```

In this example, there is a Credo config file in my home folder and one in my project.

For project `foo/`, the contents of `/home/rrrene/projects/foo/.credo.exs` are merged with the settings in `/home/rrrene/.credo.exs` and Credo's default config.

For project `bar/`, the contents of `/home/rrrene/.credo.exs` and Credo's default config are relevant.

Please not that, as mentioned above, Credo's config can also resive in a `config/` subdirectory at every step of the way.

Given this directory structure:

```shell
/
  home/
    rrrene/
      config/
        .credo.exs
      projects/
        bar/
        foo/
          config/
            .credo.exs
```

For project `foo/`, the contents of `/home/rrrene/projects/foo/config/.credo.exs` are merged with the settings in `/home/rrrene/config/.credo.exs` and Credo's default config.

This works great for umbrella projects, where you can have individual `.credo.exs` files for each app and/or a global one in the umbrella's `config/` or root folder.
This way, you can enable/disable settings on a per-app basis.


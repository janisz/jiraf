# Jiraf

Jiraf finds an issue in Jira by key and generates a git branch name from its summary.

## Configuration

Configuration file stored in `~/.config/jiraf/config.yml` in the YAML format.

```yaml
url: <url to your Jira>
username: <username>
password: <Jira API token>
format: <format for fmt.Sprintf, the first argument is an issue key, the second one is generated summary>
pipeline:
  <function>: [<arguments>...]
  ...
```

### Example

```yaml
url:  <url to your Jira>
username: <username>
password: <Jira api token>
format: "%s_%s"
pipeline:
  replace: [' ', '-']
  replace_regexp: ['[^a-zA-Z0-9-]+', '']
  to_lower:
```

This configuration will generate a branch name like this: `ISSUE-1_Summary-of-your-issue`

## Pipeline

Pipeline is a set of function with arguments, that processes issue summary to generate a branch name.

Available functions:

| Name             | Arguments     | Description                                           |
|------------------|---------------|-------------------------------------------------------|
| `to_lower`       |               | Convert a string to lowercase                         |
| `replace`        | `old, new`    | Replace all `old` with `new`                          |
| `replace_regexp` | `regexp, new` | Replace all characters that match `regexp` with `new` |

## Usage

One way to integrate jiraf with git is to create a simple script like this:

```bash
#!/bin/bash

git checkout -b $(jiraf $1)
```

And put this script into `~/bin/git-cb` file (replace "cb" with preferred subcommand name). Now you can just run `git cb ISSUE-1`, which will create a new branch with a generated name by jiraf.

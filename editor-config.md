---
title: 'Level 0 - Coding Standard with EditorConfig'
subtitle: 'Level up consistency of coding standard'
description: 'Editor Config, an INI format based configuration system that let you establish project level coding standard; It allows configuring: indentation style, indentation size, line width and more'
image: 'https://themightyprogrammer.dev/post/editor-config.jpg'
keywords: 'coding standard, editor config, consistent coding standard, editor config guide, editorconfig tutorial, editorconfig linewidth, editorconfig indentsize, intellj editorconfig'
datePublished: '2020-01-06'
lastModified: '2020-04-02'
tags: Coding standand
---

[Editor Config](https://editorconfig.org), an INI¹ format based configuration system that let you establish project level coding standard; It allows configuring: indentation style, indentation size, line width and more. It helps in reducing the effort required to bring each team member to the consistent coding standard by automatically importing and applying the configuration to IDE.

## What is EditorConfig solving?

Whenever we work in a team, we set up some ground rules to work efficiently; in the programming world, those ground rules are _coding standards_.

Coding standards may include:

**Level 0**  -  Universal File configuration: indentation style, line width, line ending, indentation size.

**Level 1**  -  Programming languages based configuration: block style, comment style, method naming, class naming.

**Level 2**  -  Project Level Workflow configuration: the process regarding development to deployment. It is more of the management side.

**_EditorConfig does support all of level-0 and some of level-1 configuration. Level 2 is out of scope for it (why?)._**

## Traditional Model of Coding Standard

Many IDEs provide a mechanism for code formatting configuration. This configuration can be exported to a file in an IDE specific file.

A team member configures; distributes configuration file within the team to avoid rework by other team members. They import the configuration file into their IDE; thus, the code standard is shared and applied.

The above exercise brings up:

- **Manual Work**: Any change in code standards leads to repeat all the process of reimporting. It involves manual work and tracing each team member on the same set of coding standards is difficult. Coding standards themselves are a long-discussed collective agreement.

- **IDE Inflexibility**: All of your team members _might not_ be using the same variant of IDE. Consequently, you have to write and maintain multiple configuration files for IDEs being in use, or all your team members have to use the same variant of IDE.

## EditorConfig saves your time by:

- **Enabling Auto Import**: Whenever there are changes in coding standards, commit the corresponding rules in `.editorconfig` file and supported IDEs automatically apply changes.

- **IDE Flexibility**: EditorConfig is available to many IDEs.
  Some IDE inherently support EditorConfig; some might require external plugins. It might be that your IDE might not support all the properties. Check the IDE documentation for details.

## In action

You create a file named `.editorconfig` in your project root. The file is collections of rules; each rule is simple key-value pair separated by `=`. The first rule you write:

```ini
root = true
```

It is a declaration that the current `.editorconfig` file is root file.

By design, editor config engine searches for `.editorconfig` file in the current working directory and its all parents' directories till it finds `.editorconfig` with `root = true` and merge the configurations of all found `.editorconfig` files.

### Apply rules to files

You can apply rules to all files or a set of files using glob patterns². The following snippet applies rules to _all the files_ in the project:

```ini
# apply rules to all files

[*] # * means all
indent_style = space #self explanatory
indent_size = 2
line_width = 110
charset = utf-8
```

### Overriding Rules

EdiorConfig files are read from top to bottom. Consequently, the latest rule became final applicable rule; this mechanism allows overriding properties.

Consider, you want to override Indention size (`indent_size`) to `4` for yaml files and keeping all the rest rules.

```ini
[*.yml]
indent_size = 4
```

YAML files would be constrained by all rules (defined in the previous section under `[*]`) and with overridden _Indention Size_.

#### Rules to Specific File

Rules can also be targeted to a specific file by addressing it with its name enclosed `[{}]` .

```ini
# Rules only applicable to kconfig.yml
[{kconfig.yml}]
indent_size = 2
```

### Merging All

By combining all discussed concepts, a .editorconfig file looks like:

```ini
root = true
[*]
indent_style = space
indent_size = 2
line_width = 110

# apply setting to yaml file

[*.yml]
indent_size = 4 #override the size 2 for 4

# Rules only applicable to kconfig.yml
[{kconfig.yml}]
indent_size = 2

# Level-1 config
[*.java]
curly_bracket_next_line = true
```

### Other Useful Rules

| Rule                    | Description                   |
| ----------------------- | ----------------------------- |
| end_of_line             | line separator                |
| max_line_length         | line width                    |
| spaces_around_operators | space around binary operators |
| indent_brace_style      | L1- block brace style         |

Check out the [official docs](https://github.com/editorconfig/editorconfig/wiki/EditorConfig-Properties) to know full supported rules.

---

### IDE Specific Rules

IDEs have been started supporting their custom rules. [IntelliJIdea](https://www.jetbrains.com/help/idea/configuring-code-style.html) introduces rules prefixed by `ij`:

| Rule                              | Description                                                                |
| --------------------------------- | -------------------------------------------------------------------------- |
| ij_visual_guides                  | Visual Guide Position                                                      |
| ij_formatter_off_tag              | comment to mark formatter off Default: `// @formatter:off`                 |
| ij_formatter_on_tag               | comment to mark formatter on Default: `// @formatter:off`                  |
| ij_formatter_tags_enabled         | enable or disable the above formatter tags                                 |
| ij_wrap_on_typing                 | enable or disable wrapping while typing                                    |
| ij_continuation_indent_size       | array elements, method chaining indentation size if continued to next line |
| ij_smart_tabs                     | enable or disable smart tabs                                               |
| ij_java_blank_lines_after_imports | enable or disableAfter import line                                         |
| .                                 | .                                                                          |
| .                                 | .                                                                          |

[Visual Studio](https://docs.microsoft.com/en-us/visualstudio/ide/editorconfig-code-style-settings-reference?view=vs-2019) also supports customised rules:

| Rule                                                                                                                  | Description |
| --------------------------------------------------------------------------------------------------------------------- | ----------- |
| dotnet_style_qualification_for_field                                                                                  | -           |
| dotnet_style_require_accessibility_modifiers                                                                          | -           |
| dotnet_style_collection_initializer                                                                                   | -           |
| dotnet_style_coalesce_expression                                                                                      | -           |
| .                                                                                                                     | .           |
| .                                                                                                                     | .           |
| and [more](https://docs.microsoft.com/en-us/visualstudio/ide/editorconfig-code-style-settings-reference?view=vs-2019) |

> If you are using IDE specific rules, you are giving away IDE flexibility benefit 🙇.

## Closing notes

EditorConfig saves your team time by automatically importing configuration. It helps in reducing the effort to bring each team member to the same definition of coding standards whenever there are changes in code standards.

## Reference

1. INI Format  
   https://en.wikipedia.org/wiki/INI_file

## Footnotes

2. GlobPattern: A algebraic way to specify a set of files.

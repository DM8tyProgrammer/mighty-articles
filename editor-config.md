---
title: Level 0 - Coding Standard with EditorConfig
description: 'Editor Config, an INI format based configuration system that let you establish project level coding standard; It allows configuring: indentation style, indentation size, line width and more'
image: '/post/editor-config.jpg'
keywords: 'coding standard, editor config, consistent coding standard, editor config guide, editorconfig tutorial'
datePublished: '2020-06-2020'
---

[Editor Config](https://editorconfig.org), an INI¹ format based configuration system that let you establish project level coding standard; It allows configuring: indentation style, indentation size, line width and more.

## What is EditorConfig solving?

Whenever we work in a team, we set up some ground rules to work efficiently; in the programming world, those ground rules are coding standard.

Coding standards may include:

**Level 0**  -  Universal file text configuration: indentation style, line width, line ending, indentation size.

**Level 1**  -  Programming languages based: block style, comment style, method naming, class naming.

**Level 2**  -  Project Level Workflow: the process regarding development to deployment. It is more of the management side.

**_EditorConfig does support all of level-0 and some of level-1 configuration. Level 2 is out of scope for it (?)._**

## Traditional Model of Coding Standard

Many IDEs provide a mechanism for code formatting configuration. This configuration can be exported to a file in an IDE specific file.

Someone in your team configures and the exported file within the team to avoid rework by them. Team members import the configuration file into their IDE. Thus, the code standard is shared and applied.

The above process brings up:

- **Manual Work**: Any change in the code standard; all the process of reimporting has to be repeated. It involves manual work and bringing each one IDE on the same page is difficult. Coding standard itself is a long-discussed collective agreement, and then there is manual work; It is an invisible pain of a developer.

- **Single Typed IDE**: All of your team members might not be using the same IDE. Consequently, you have to write and maintain multiple configuration files for IDEs being in use, or all your team members have to use the same variant of IDE.

## EditorConfig saves your time by:

- **Enabling Auto Import**: Whenever there is change, commit in .editorconfig file and supported IDEs automatically apply changes.
- **IDE agnostic** : EditorConfig is available to multiple IDEs. 
  Some IDE inherently support EditorConfig; some might require external plugins. It might be that your IDE might not support all the properties. Check the IDE documentation for details.

## In action

You create a file named `.editorconfig` in your project root. The file is collections of rules; each rule is simple key-value pair separated by `=`.

The first rule you write:

```ini
root = true
```

It is a declaration that the current file is root `.editorconfig` file.

By design, editor config engine/interpreter searches for `.editorconfig` file in the open directory and its all parents' directories till it find `.editorconfig` with `root = true`.

### Apply properties to files

You can apply rules to all files or a set of files. It supports glob patterns² to target files.

```ini
# apply rules to all files

[*] # \* means all
indent_style = space
indent_size = 2
line_width = 110
charset = utf-8
```

EdiorConfig files are read from top to bottom. It means the latest rule is considered the final applicable rule and this can be used to override properties.

```ini
[*.yml]
indent_size = 2

# yaml would get all value with overridden indent_size
```

By combing all, a typical file look like as:

```ini
root = true
[*]
indent_style = space
indent_size = 2
line_width = 110

# apply setting to yaml file

[*.yml]
indent_size = 4 #override the size 2 for 4

# Level-1 config

[*.java]
curly_bracket_next_line = true
```

Check out the [official docs](https://github.com/editorconfig/editorconfig/wiki/EditorConfig-Properties) to know full supported properties.

---

EditorConfig saves your team time by automatically importing configuration. It helps in reducing the effort to bring each team member to the same definition of coding standard whenever there is a change in "code standard".

## Reference

1. INI Format  
   https://en.wikipedia.org/wiki/INI_file

## Footnotes

2. GlobPattern: A algebraic way to specify a set of files.

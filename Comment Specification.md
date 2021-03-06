---
Version: 0.1.0
---

# Comment Specification

Comments are regular git objects, stored in a format similar to tags.

## Field Types

The fields of a comment are formatted as:
* A single keyword followed by a space and the value, or
* A single keyword ending in a colon followed by subsequent lines prefixed by an
  asterisk.

## Field Enumeration

The fields of a comment are (in order):

1. `commit` - Full hash of the commit to which the comment is attached.
    Required to be not empty.
2. `file` - File path relative to the root of the git repository and
    line number, separated by a colon. The file key is required, but the
    value can be empty.
3. `author` - The writer of the comment's name, email, and time of
    creation. Time is formatted as a Unix timestamp followed by a single
    space and the timezone in the format `+HHMM` or `-HHMM`. The full author
    format is `User A Name <name@example.com> [Unix timestamp] [timezone]`.
    Required to not be empty.
4. `amender` - Person who committed the comment into git, in the same
    format as the author. Required to not be empty.
5. `links:` - One or many reference URLs relating to the comment. Identifiers
    and URLs follow on subsequent lines preceded by an asterisk in the format

        * [identifier] <[URL]>

    where `identifier` is a string composed of alphanumeric characters, dashes,
    spaces, and underscores, which must be unique in the collection of links.
    URL must be fully qualified with the protocol. The link key is optional.

After the fields, there is a single empty line, then the content of the
comment, which can be multiple lines.

Example comment:

```
commit 0155eb4229851634a0f03eb265b69f5a2d56f341
file src/main.c:12
author Delisa Mason <name@example.com> 1243040974 -0900
amender Delisa Mason <name@example.com> 1243040974 -0900
links:
* Synchronization URL <https://example.com/project/issues/14/comments/123>
* Related RFC <https://example.com/project/proposals/98>

Too many levels of indentation here.

Consider updating the linter as well as a part of this changeset.
```

## Comment Reference Specification

A reference for each comment is added in `refs/comments` for lookup
by the commit and to prevent object garbage collection. The content of the
reference is the comment hash.

The reference path format is:

```
refs/comments/[first four character of commit hash]/[rest of commit]/[comment hash]
```

Example:

```
refs/comments/0155/eb4229851634a0f03eb265b69f5a2d56f341/f9da8cdd40bbce4c7bd1aa4e46608107184bd91c
```

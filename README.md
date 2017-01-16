# gmail-yaml-filters

A quick tool for generating Gmail filters from YAML rules.

## Getting Started

Quick start:

```
pip install gmail-yaml-filters
gmail-yaml-filters my-filters.yaml > my-filters.xml
```

(Will add to PyPI soon. It's not up there yet.)

## Sample Configuration

```yaml
# Simple example
-
  from: googlealerts-noreply@google.com
  label: news
  not_important: true

# Boolean conditions
-
  from:
    any:
      - alice
      - bob
      - carol
  to:
    all: [me, -MyBoss]
  label: conspiracy

# Nested conditions
-
  from: lever.co
  label: hiring
  more:
    -
      has: 'completed feedback'
      archive: true
    -
      has: 'what is your feedback'
      star: true
      important: true

# Foreach loops
-
  for_each:
    - list1
    - list2
    - list3
  rule:
    to: "{item}@mycompany.com"
    label: "{item}"

# Foreach loops with complex structures
-
  for_each:
    - [mailing-list-1a, list1]
    - [mailing-list-1b, list1]
    - [mailing-list-1c, list1]
    - [mailing-list-2a, list2]
    - [mailing-list-2b, list2]
  rule:
    to: "{item[0]}@mycompany.com"
    label: "{item[1]}"
```

## Configuration

Supported conditions:

* `does_not_have` (also `missing`, `no_match`)
* `from`
* `has` (also `match`)
* `list`
* `subject`
* `to`

Supported actions:

* `archive`
* `important` (also `mark_as_important`)
* `label`
* `not_important` (also `never_mark_as_important`)
* `not_spam`
* `read` (also `mark_as_read`)
* `star`
* `trash` (also `delete`)

Any set of rules with `ignore: true` will be ignored and not written to XML.

## Similar Projects

* [gmail-britta](https://github.com/antifuchs/gmail-britta) is written in Ruby and lets you express rules with a DSL.
* [gmail-filters](https://github.com/dimagi/gmail-filters) is written in Python and has a web frontend.

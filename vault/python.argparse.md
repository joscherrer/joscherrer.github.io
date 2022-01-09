---
id: uvOyqp7f7sj2CxwroIGXx
title: Argparse
desc: ''
updated: 1641751952635
created: 1641751672406
---

## Basic argparse setup

```python
import argparse

parser = argparse.ArgumentParser()
parser.add_argument("-f", "--flag", action='store_true') # Flag (default: false)
parser.add_argument("-i", "--item", type=str) # Optional
parser.add_argument("x", type=str) # Positional
parser.parse_args()
```

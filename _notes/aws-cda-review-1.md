---
title: AWS Cloud Formation
layout: post
tags: [aws, cda]
date: 2020-09-14
---

### Introduction

- infrastracture as code, easy to recreate
-  declarative way, pseudo code
-  no manual, versioned by git, changes by code
- can track cost of a stack, saving strategy creating,stoping stack
- no need to orchestrace create, separation of concerns (VPC, network, app)
- using templates online, also useful for generate documentation
- template componentents: resources(mandatory), parametars, mapping, outputs, condiditionals, metadata
- update and version of templates
- YAML format, key pair values, nested values, arrays (with "-") or list, multi line string (with '|')  



## AWS reference types
- https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html
- no dynaminc amount of resources, everything declared
- aws service is supported, only a few not yet
- reference of param or resource exp: !Ref MyVPC . concept of pseudo params
- mappings are fixed variables withing template. differentiate between environments, regions, types. values are hardcoded
- !FindInMap [ MapName, ToplevleKey, SecondLevelKey] - returns a value from specific key
- outputs are optional, we can import into other stacks if exported. cross stack colaboration. Export: Name: ValueName . !ImportValue ValueName .
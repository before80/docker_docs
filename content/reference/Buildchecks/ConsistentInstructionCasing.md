+++
title = "ConsistentInstructionCasing"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/build-checks/consistent-instruction-casing/](https://docs.docker.com/reference/build-checks/consistent-instruction-casing/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# ConsistentInstructionCasing

## [Output](https://docs.docker.com/reference/build-checks/consistent-instruction-casing/#output)



```text
Command 'EntryPoint' should be consistently cased
```

## [Description](https://docs.docker.com/reference/build-checks/consistent-instruction-casing/#description)

Instruction keywords should use consistent casing (all lowercase or all uppercase). Using a case that mixes uppercase and lowercase, such as `PascalCase` or `snakeCase`, letters result in poor readability.

## [Examples](https://docs.docker.com/reference/build-checks/consistent-instruction-casing/#examples)

❌ Bad: don't mix uppercase and lowercase.



```dockerfile
From alpine
Run echo hello > /greeting.txt
EntRYpOiNT ["cat", "/greeting.txt"]
```

✅ Good: all uppercase.



```dockerfile
FROM alpine
RUN echo hello > /greeting.txt
ENTRYPOINT ["cat", "/greeting.txt"]
```

✅ Good: all lowercase.



```dockerfile
from alpine
run echo hello > /greeting.txt
entrypoint ["cat", "/greeting.txt"]
```

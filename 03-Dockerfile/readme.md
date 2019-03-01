## Dockerfile

##### Introduction

- contains sequential sets of instructions.
- Prime way to interact with docker daemon.
- Each instruction creates a layers.
- A stack of such sequenced layers managed by a file system called **docker image**.
- So it has enabled caching and troubleshooting facility.
- Docker daemon can reuse the pre-created layer for a docker file layer.


###### Structure
---

**Rule of Thumb : The name should be `Dockerfile`** 

**Fundamental Instructions**
**Configuration Instructions**
**Execution Instructions**
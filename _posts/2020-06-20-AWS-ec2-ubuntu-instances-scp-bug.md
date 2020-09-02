---
published: true
layout: post
author: eugene
categories: cloud
tags:
- featured
- AWS
- EC2
---
## AWS EC2 default `~/.bashrc` will cause error on SCP.

Sometimes we want to scp file to AWS EC2 ubuntu instances.
But we could face problem where the `~/.bashrc` does not work properly. Particularly SCP failed to output even any error. Since I have faced this problem several times I decided to post this here.

`vim ~/.bashrc`

hit `I` to go into inser mode of vim

comment
```bash
.
.
.
case $- in
    *i*) ;;
      *) return;;
esac
.
.
.
```

So the result should look like
```bash
.
.
.
#case $- in
#    *i*) ;;
#      *) return;;
#esac
.
.
.
```

Now scp should work properly!



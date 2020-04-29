---
published: false
layout: post
categories:
  - Python
  - Util
  - Unix
  - IPython
---
## iPython Autosave

Though Jupyter Lab/Notebook are great they never truly gets me to the level of IPython. I still prefer using IPython in alot of cases. It's like Python from Apple XD.

But sometimes I wish that the code I previously tested on IPython able to be search back. Because I mostly use IPython for small utilities package generating testing. 

```bash
alias ipython='ipython --logfile="ipython_$(date +%y%m%d_%H%m).py"'
```

> This alias makes ipython automatically save files on every session of ipython.

But don't change the file content when the session is still online. It will break the ipython writes and ipython will stop writing onto that file.

Hopes that this can help you next time. Sometimes we need to find back our scratch files to recall how we do something.

Bash command is always like an alien language to me. But `alias`es is really powerful I hope I can write more about these in the future.

\* TIP: Another useful command that most people might not use is `cd -` this returns you to your previous directory if you have changed any!

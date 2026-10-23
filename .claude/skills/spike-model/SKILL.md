---
name: spike-model
description: A.2 spike of the fab0 rebuild. Checks whether a skill's model setting takes effect in a routine session. Use only when a fab0 attempt brief names it.
model: haiku
---

# spike-model

You are running one fab0 attempt. Its brief gave you an attempt reference, such as `r1-spike-abc123`.

1. Write the file `spike/<reference>-model.md` at the repository root, with exactly these lines:

   ```
   # Spike attempt <reference>: model check

   This skill asks for the haiku model class. The model running it says it is: <your model name, as you know it>.
   ```

2. Don't commit, push or open a pull request: `fab0 attempt finish` does that.
3. Stop, and report the path you wrote.

The transcript records which model actually ran each message; `fab0 attempt finish` reads it from there.

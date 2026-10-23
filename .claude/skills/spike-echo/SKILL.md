---
name: spike-echo
description: A.2 spike of the fab0 rebuild. Writes one small file for a fab0 attempt, so the attempt has a change to commit. Use only when a fab0 attempt brief names it.
---

# spike-echo

You are running one fab0 attempt. Its brief gave you an attempt reference, such as `r1-spike-abc123`.

1. Write the file `spike/<reference>.md` at the repository root, with exactly these lines:

   ```
   # Spike attempt <reference>

   Written by the spike-echo skill at <the current UTC time, ISO 8601>.
   ```

2. Don't commit, push or open a pull request: `fab0 attempt finish` does that.
3. Stop, and report the path you wrote.

# bnudiff - *B*usyBox *N*on-*U*nified *diff*

If you're on a system with BusyBox utilities (phones, routers, Alpine Linux,
etc), you've probably noticed that its `diff` only produces unified diffs. The
unified diff format is great of course, but there are various tools that
unportably assume `diff` will output traditional/"normal" diffs by default.

bnudiff is a script that BusyBox-based and similar systems can use to produce
the traditional/"normal" diff output format.

Pull requests welcome. See below for ideas if you want to contribute.



### Deficiencies

Exit status of `diff` is currently not preserved.
The intent is to fix this once there's time, especially if there's demand.



### Dependencies

This wrapper currently depends on the following for the basic logic:

* `diff -U0` (support for the `-U` option is not a hard requirement,
  as context lines can be trivially stripped by `sed` or `awk`, but obviously
  the output must be unified diffs).
* `awk` (most of the logic happens here. I've seen some lobotomized phones
  not have `awk` installed, but this should be fairly universal)

It also depends on these two programs to succesfully parse binary data (needed
to correctly convert "treat all files as text" diff output).

* `sed` (BusyBox `awk` confuses null bytes for record separators,
  while BusyBox's `sed` can cleanly pass null bytes through unaltered.)
* `tr` (BusyBox `sed` can't actually replace null bytes on the builds tested,
  while BusyBox `tr` handles null bytes without issue.)

There are possible workarounds for all of these dependencies in principle,
But they are convoluted, obscenely inefficient, and bloat the code with
numerous run-time checks for other available features.
Might be worth implementing as a fallback if people need this though.



### Limitations

* Currently, this has only been tested with BusyBox `diff`, `awk`, `tr`, and
  `sed`. Since the only `diff` I know of that produces unified diffs by default
  but cannot produce normal traditional diffs is BusyBox's, this isn't a major
  concern for me.
* No intelligent option parsing,
  so if you invoke it with "-u" or "-U" you won't get unified output.
* No conversion to context diff - just to traditional.
  It's a slightly more complex problem that I'm not sure is worth working on.
  But feel free to let me know if you need this.

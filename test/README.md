## No Tests Here

There's no actual test scripts, etc, here. Just two pairs of files for testing
against. Proper testing would involve a fuzzer or something, or at least a more
involved/rigorous/taxing set of files to diff.

Still, a.txt and b.txt differ in every distinct way (changed lines, removed
lines, added lines) that's relevant for the conversion. And a.bin has a null
byte in the contents where b.bin doesn't, allowing for verification that the
"treat all files as text" option works correctly as well.

Pull requests with proper tests are welcome.

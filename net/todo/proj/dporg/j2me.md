# Java ME notes

## Versions

Java ME 3.0 is the latest and it's constrained to Java 1.3,
so > -source 1.3 errors are par for the course,
just fix the stupid shit the decompilers output.

Wrt Netbeans, right now using v6,
closest to what might've been during build.
Netbeans 7 might've worked on last attempt, dunno.
No reasons to switch right now.


## Starting over from scratch

### Netbeans
- New project, Java ME with existing sources
- Separate src and dat directory,
esp if we'll try other shit
- Can point at a manifest (*.mf) if we have one


### Working test case
- A test project can just be using the compiled class files,
with same setup as from scratch
- The emulator does work on that,
and it can be used as a simple test of the whole system


### Decompilation tests
- Another test is a decompilation with minimum changes
- Every decompiler will have lots of errors


### Fixing compilation
- there's no way to distinguish two functions with the same arguments
but a different return type
(esp when their returned values aren't assigned)
- int cannot be dereferenced:
probably collision between class name
and current class variables;
usually just renaming the class is fine,
netbeans will fix known references,
then have to fix manually all the other ones
- -source 1.5 needed:
get code from the latest annotated version

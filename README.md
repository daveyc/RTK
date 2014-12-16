RTK
===

The z/OS REXX toolkit is a command processor that provides functionality not provided by standard z/OS REXX APIs. At present
RTK only supports regular expressions. Future releases may add file I/O, including VSAM which is a sorely missing from z/OS REXX.

RTK regex uses ECMAScript grammar http://www.ecma-international.org/ecma-262/5.1/#sec-A.7.

```rexx
/* REXX */

  address LINK 'RTKSUBCM' /* initialize the glue! */

  address RTK

  /* search for and split an IP address */
  ipaddr = "(\d{1,3})\.(\d{1,3})\.(\d{1,3})\.(\d{1,3})"

  "REGEX COMPILE ID(IP) PATTERN('"ipaddr"')"

  v = '192.168.1.1'

  "REGEX SEARCH ID(IP) INPUT(V) RESULTS(MR.)"
  if rc = 0 then do
    do i = 1 to mr.0
      say "pos="mr.i.pos "str="mr.i
    end
  end
  else say 'No match'

  /* validate a data set name */
  dsname = "^[a-zA-Z#$@][a-zA-Z0-9#$@-]{0,7}([.][a-zA-Z#$@]" ||,
           "[a-zA-Z0-9#$@-]{0,7}){0,21}"

  "REGEX COMPILE ID(REG1) PATTERN('"dsname"')"

  v = 'DOC.DEVT.CPP'

  "REGEX MATCH ID(REG1) INPUT(V) RESULTS(MR.)"
  if rc = 0 then say 'Valid data set name'
  else           say 'Invalid data set name'


  /* re-format a date */
  date = "(\d{1,2})(\.|-|/)(\d{1,2})(\.|-|/)(\d{4})"

  "REGEX COMPILE ID(DATE) PATTERN('"date"')"

  v = '01/02/2008 03/02/2000 11/10/1970'

  "REGEX REPLACE ID(DATE) INPUT(V) FORMAT('$5-$3-$1')"

  say v
```

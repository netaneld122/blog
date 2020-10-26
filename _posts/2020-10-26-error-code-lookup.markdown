---
layout: post
title:  "Looking up for Windows API error codes - tools and methods"
date:   2020-10-26
---

## Offline tools

### net helpmsg
This is a built-in Windows executable called net.exe that allows you to quickly search for exact error codes.

For example:
```ps
net helpmsg 5
```
Will output:
```
Access is denied.
```

### err.exe (Microsoft Error Lookup Tool)
This tool can be [downloaded from Microsoft's website][err], it's more versatile than `net helpmsg` because it matches partial error codes and ambiguous values.  
Additionally it provides useful command line arguments for your convenience:

```
USAGE: err [opt] {value} [value] [value] ...
 where <value> must be of one of the following forms:
   1. decorated hex (0x54f)
   2. implicit hex  (54f)
   3. ambiguous     (1359)
   4. exact string  (=ERROR_INTERNAL_ERROR)
   5. substring     (:INTERNAL_ERROR)
...and <opt> may be one of:
   /:xml         - causes the output to be in XML-parseable form.
                   To understand the output, try it.  It's pretty obvious.
   /:listTables  - lists all the tables below in XML format.
                   Again, the format is pretty straightforward.
   /:outputtoCSV - lists all the tables below in CSV format.
   /:outputtoJS  - lists all the tables below for use in JS.
   /:outputtoCPP - lists all the tables below for a C++ header.
   /:hresultfromwin32 - prints HRESULT_FROM_WIN32 errors for a C++ header.
```
For example:
```ps
err /winerror.h 5
```
Will output:
```
# winerror.h selected.
# for hex 0x5 / decimal 5
  ERROR_ACCESS_DENIED                                            winerror.h
# Access is denied.
```

### Windbg
If you're using a debugger, and you encounter an error code during your debugging session, the `!error` extension might be useful.

For example:
```ps
!error 5
```
Will output:
```
Error code: (Win32) 0x5 (5) - Access is denied.
```

## Online tools

### [error.guru](https://error.guru)

This is a website I created based on `err.exe`'s internal database by exporting it to a CSV file and wrapping it all up with some fancy Vue.js components.

You can search for hex values, decimal values and substrings of the error code names.

### Other websites

* [https://james.darpinian.com/decoder/](https://james.darpinian.com/decoder/)


[err]: https://www.microsoft.com/en-us/download/details.aspx?id=100432
---
layout: page
title: Doxygen
---
# Doxygen

## How-To: Generate Call Graph for Fortran Application

* Generate Base Doxyfile:
```
$ doxygen -g
```

* Edit the Doxyfile by Setting the Following Values:
  * Project Name
  ```
  PROJECT_NAME =
  ```
  * Input Directory:
  ```
  INPUT = <Input Dir>
  ```
  * Fortran Code Settings:
  ```
  FILE_PATTERNS = .f90
  OPTIMIZE_FOR_FORTRAN = YES
  ```
  * Extraction Settings:
  ```
  EXTRACT_ALL = YES
  EXTRACT_PRIVATE = YES
  EXTRACT_STATIC = YES
  ```
  * Dot Utility Configuration:
  ```
  HAVE_DOT = YES
  DOT_PATH = <Path>
  ```
  * Call Graph Settings:
  ```
  CALL_GRAPH = YES
  CALLER_GRAPH = YES
  ```
  * Output Format Settings:
  ```
  GENERATE_HTML = YES
  GENERATE_LATEX = NO
  ```

* Run Generation:
```
$ doxygen Doxyfile
```

> Note: Output files are created by default in subdirectories.

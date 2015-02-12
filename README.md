# Process SWI-Prolog HTTP logfiles

The SWI-Prolog HTTP server can be asked   to  write a logfile by loading
the library(http/http_log). The resulting file is  a file holding Prolog
terms. This package allows  for  loading   the  logfile  into the Prolog
database and ask queries about it.

## Loading a logfile

Before you can analyse  a  logfile  it   must  be  read  into the Prolog
database. Assuming the default name `httpd.log`,  this is achieved using
the following command:

   ```
   % swipl
   ?- use_module(library(http/logstat)).
   ?- read_log('httpd.log').
   ```

## Query the log database

In  the  examples  below,   we   assume    a   logfile   as  created  by
[ClioPatria](http://cliopatria.swi-prolog.org).  Adjust  the  paths  and
query parameter to match your web server.  Once loaded, the database can
be queried using logrecord/1. A simple example is:

  ```{prolog}
  ?- logrecord([path('/sparql/'), search([query=Query])]).
  ```

### Make a CSV file

Suppose you want to make a CSV  file   holding  the IP addresses and the
search executed from there. This can be achieved using:

  ```{prolog}
  ?- findall(row(IP,Q),
             logrecord([path('/browse/search'), search([q=Q]), ip(IP)]),
             Rows),
     csv_write_file('queries.csv', Rows).

  ```

## Converting lofiles

The  Prolog  script  file  script/log2clf.pl  can  be  used  to  convert
SWI-Prolog logfiles into the webserver _Common   Log  Format_ syntax, so
you can run common tools  such  as   webalizer  on  them. This script is
executed as:

  ```
  % log2clf [-o out] [-a date] [-b date] file ...
  ```

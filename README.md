# utl-cast-character-variables-to-numeric-with-the-same-names
Cast character variables to numeric with the same names
    Cast character variables to numeric with the same names

        Four Solutions ( two sets: one simpler with warnings and one without warnings)

              1. Change just character variables that begin with 'R' to numeric (with and without warnings)
              2. Change all character variables to numeric (with and without warnings)

        This solution use

            varlist macro by  Søren Lassen, s.lassen@post.tele.dk

            array and do_over macros by
               tclay@ashlandhome.net  (541) 482-6435
               David Katz, M.S. www.davidkatzconsulting.com

           Bart Jablonski  (has added a dataset functionality, missing in the macro language to do_over and array)
           Bartosz Jablonski yabwon@gmail.com


    github
    https://tinyurl.com/y3w8zlcb
    https://github.com/rogerjdeangelis/utl-cast-character-variables-to-numeric-with-the-same-names

    SAS Forum
    https://tinyurl.com/yykfxq56
    https://communities.sas.com/t5/SAS-Procedures/Can-I-change-formats-of-several-variabels-in-one-procedure/m-p/558893

    macros
    https://tinyurl.com/y9nfugth
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories



    *_                   _
    (_)_ __  _ __  _   _| |_
    | | '_ \| '_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    ;

    options validvarname=upcase;

    data have;

      array  chrs[7] $11 aptNum ssn pinNum age r1 r2 r9 ( '707' '123456789' '0123' '55' '91' '92' '93' );
      array  num[2] height weight  ( 60 110  );

    run;quit;

             WORK.HAVE
             =========

      Variable    Type    Len

      APTNUM      Char     11
      SSN         Char     11
      PINNUM      Char     11
      AGE         Char     11

      R1          Char     11
      R2          Char     11
      R9          Char     11

      HEIGHT      Num       8
      WEIGHT      Num       8

    WORK.HAVE total obs=1


                Character variables to chage to  numeric
          ------------------------------------------------------
    Obs    APTNUM      SSN       PINNUM    AGE    RR    R2    R9    HEIGHT    WEIGHT

     1      707      12345678     0123     55     91    92    93      60        110

    *           _
     _ __ _   _| | ___  ___
    | '__| | | | |/ _ \/ __|
    | |  | |_| | |  __/\__ \
    |_|   \__,_|_|\___||___/                              SOLUtons

             WORK.HAVE           |  SOLUTION 1(Change R Vars) |   Change all Character vars
             =========           |                            |
                                 |                            |
      Variable    Type    Len    |   Variable    Type    Len  |    Variable    Type    Len
                                 |                            |
      APTNUM      Char     11    |   APTNUM      Char     11  |    APTNUM      Num       8
      SSN         Char     11    |   SSN         Char     11  |    SSN         Num       8
      PINNUM      Char     11    |   PINNUM      Char     11  |    PINNUM      Num       8
      AGE         Char     11    |   AGE         Char     11  |    AGE         Num       8

      R1          Char     11    |   R1          Num       8  |    R1          Num       8
      R2          Char     11    |   R2          Num       8  |    R2          Num       8
      R9          Char     11    |   R9          Num       8  |    R9          Num       8

      HEIGHT      Num       8    |   HEIGHT      Num       8  |    HEIGHT      Num       8
      WEIGHT      Num       8    |   WEIGHT      Num       8  |    WEIGHT      Num       8

    *            _               _
      ___  _   _| |_ _ __  _   _| |_ ___
     / _ \| | | | __| '_ \| | | | __/ __|
    | (_) | |_| | |_| |_) | |_| | |_\__ \
     \___/ \__,_|\__| .__/ \__,_|\__|___/
                    |_|
    ;

     Four tables

     SOLUTION 1(Change R Vars) |   Solution 2: Change all Character vars
                               |
     WORK.WANT_R_WARN          |   WORK.WANT_ALL_WARN
     WORK.WANT_R_NOWARN        |   WORK.WANT_ALL_NOWARN
                               |
     SOLUTION 1(Change R Vars) |   Change all Character vars
                               |
                               |
      Variable    Type    Len  |    Variable    Type    Len
                               |
      APTNUM      Char     11  |    APTNUM      Num       8
      SSN         Char     11  |    SSN         Num       8
      PINNUM      Char     11  |    PINNUM      Num       8
      AGE         Char     11  |    AGE         Num       8

      R1          Num       8  |    R1          Num       8
      R2          Num       8  |    R2          Num       8
      R9          Num       8  |    R9          Num       8

      HEIGHT      Num       8  |    HEIGHT      Num       8
      WEIGHT      Num       8  |    WEIGHT      Num       8


    *
     _ __  _ __ ___   ___ ___  ___ ___
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    ;

    options validvarname=upcase;

    data have;

      array  chrs[7] $11 aptNum ssn pinNum age r1 r2 r9 ( '707' '123456789' '0123' '55' '91' '92' '93' );
      array  num[2] height weight  ( 60 110  );

    run;quit;


    ************************************************************************************
    1. Change just character variables that begin with 'R' to numeric                  *
    ************************************************************************************



    WITH WARNINGS

    %array(change,values=%varlist(have,prx=/^R/i));   * variables that begin with R;

    proc sql;
      create
         table WORK.WANT_R_WARN as
      select
         %do_over(change,phrase=input(?,best32.) as ?,between=comma)
        ,*
      from
         have
    ;quit;

    SQL ignores second original type
    WARNING: Variable R1 already exists on file WORK.WANT_R_WARN.
    WARNING: Variable R2 already exists on file WORK.WANT_R_WARN.
    WARNING: Variable R9 already exists on file WORK.WANT_R_WARN.




    WITHOUT WARNINGS

    %array(change,values=%varlist(have,prx=/^R/i));         * variables that begin with R;
    %array(nochange,values=%varlist(have,prx=/^(?!(R))/i)); * variables that do not begin with R;

    %put &=nochange2;

    proc sql;
      create
         table want_%_NOWARN as
      select
         %do_over(change,phrase=input(?,best32.) as ?,between=comma)
         %do_over(nochange,phrase=%str(,?))
      from
         have
    ;quit;


    *************************************************************************
    2. Change all character variables to numeric (with and without warnigs) *
    *************************************************************************



    WITH WARNINGS


    %array(chrs,values=%varlist(have,keep=_character_));

    proc sql;
      create
         table want as
      select
         %do_over(chrs,phrase=input(?,best32.) as ?,between=comma)
        ,*
      from
         have
    ;quit;

    WARNING: Variable APTNUM already exists on file WORK.WANT.
    WARNING: Variable SSN already exists on file WORK.WANT.
    WARNING: Variable PINNUM already exists on file WORK.WANT.
    WARNING: Variable AGE already exists on file WORK.WANT.
    WARNING: Variable R1 already exists on file WORK.WANT.
    WARNING: Variable R2 already exists on file WORK.WANT.
    WARNING: Variable R9 already exists on file WORK.WANT.



    WITHOUT WARNINGS


    %array(chrs,values=%varlist(have,keep=_character_));   * character variables ;
    %array(nums,values=%varlist(have,keep=_numeric_));      * numeric variables   ;

    proc sql;
      create
         table want_all_nowarn as
      select
         %do_over(chrs,phrase=input(?,best32.) as ?,between=comma)
         %do_over(nums,phrase=%str(,?))
      from
         have
    ;quit;


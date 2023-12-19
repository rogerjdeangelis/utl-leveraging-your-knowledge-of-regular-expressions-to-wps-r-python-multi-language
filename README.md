# utl-leveraging-your-knowledge-of-regular-expressions-to-wps-r-python-multi-language
    %let pgm =utl-leveraging-your-knowledge-of-regular-expressions-to-wps-r-python-multi-language;

    Leveraging your knowledge of regular expressions to wps r python multi language;

    github
    https://tinyurl.com/59bx7me8
    https://github.com/rogerjdeangelis/utl-leveraging-your-knowledge-of-regular-expressions-to-wps-r-python-multi-language

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                                          |                                 |                                           */
    /*                                          |                                 |                                           */
    /*                 INPUT                    |            PROCESS              |                    OUTPUT                 */
    /*                                          |                                 |                                           */
    /* NAME       SEX  AGE    HEIGHT    WEIGHT  |  select names that begin with J |  NAME       SEX  AGE    HEIGHT    WEIGHT  */
    /*                                          |                                 |                                           */
    /* Alfred      M    14     69.0      112.5  |  if prxmatch("/^J/",name);       |  James       M    12     57.3       83.0  */
    /* Alice       F    13     56.5       84.0  |                                 |  Jane        F    12     59.8       84.5  */
    /* Barbara     F    13     65.3       98.0  |                                 |  Janet       F    15     62.5      112.5  */
    /* Carol       F    14     62.8      102.5  |                                 |  Jeffrey     M    13     62.5       84.0  */
    /* Henry       M    14     63.5      102.5  |                                 |  John        M    12     59.0       99.5  */
    /* James       M    12     57.3       83.0  |                                 |  Joyce       F    11     51.3       50.5  */
    /* Jane        F    12     59.8       84.5  |                                 |  Judy        F    14     64.3       90.0  */
    /* Janet       F    15     62.5      112.5  |                                 |                                           */
    /* Jeffrey     M    13     62.5       84.0  |                                 |                                           */
    /* John        M    12     59.0       99.5  |                                 |                                           */
    /* Joyce       F    11     51.3       50.5  |                                 |                                           */
    /* Judy        F    14     64.3       90.0  |                                 |                                           */
    /* Louise      F    12     56.3       77.0  |                                 |                                           */
    /* Mary        F    15     66.5      112.0  |                                 |                                           */
    /* Philip      M    16     72.0      150.0  |                                 |                                           */
    /* Robert      M    12     64.8      128.0  |                                 |                                           */
    /* Ronald      M    15     67.0      133.0  |                                 |                                           */
    /* Thomas      M    11     57.5       85.0  |                                 |                                           */
    /* William     M    15     66.5      112.0  |                                 |                                           */
    /*                                          |                                 |                                           */
    /**************************************************************************************************************************/


    SOLUTIONS

     1 wps datastep
       if prxmatch("/^J/",name);

     2 wps r dataframe
       have |> filter(grepl('^J', NAME));  (similar to wps)

     3 wps python dataframe
       have[have.NAME.str.match('^J')]  (different from wps/sas and r)

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */
    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
      set sashelp.class; ;
    run;quit;

    /*                            _       _            _
    / | __      ___ __  ___    __| | __ _| |_ __ _ ___| |_ ___ _ __
    | | \ \ /\ / / `_ \/ __|  / _` |/ _` | __/ _` / __| __/ _ \ `_ \
    | |  \ V  V /| |_) \__ \ | (_| | (_| | || (_| \__ \ ||  __/ |_) |
    |_|   \_/\_/ | .__/|___/  \__,_|\__,_|\__\__,_|___/\__\___| .__/
                 |_|                                          |_|
    */

    proc datasets lib=sd1 nolist mt=data mt=view nodetails;delete want; run;quit;

    %utl_submit_wps64x('
    libname sd1 "d:/sd1";
    data sd1.want;
      set sd1.class;
      if prxmatch("/^J/",name);
    run;quit;
    ');

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*   NAME       SEX    AGE    HEIGHT    WEIGHT                                                                            */
    /*                                                                                                                        */
    /*   James       M      12     57.3       83.0                                                                            */
    /*   Jane        F      12     59.8       84.5                                                                            */
    /*   Janet       F      15     62.5      112.5                                                                            */
    /*   Jeffrey     M      13     62.5       84.0                                                                            */
    /*   John        M      12     59.0       99.5                                                                            */
    /*   Joyce       F      11     51.3       50.5                                                                            */
    /*   Judy        F      14     64.3       90.0                                                                            */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                                    _       _         __
    |___ \  __      ___ __  ___   _ __    __| | __ _| |_ __ _ / _|_ __ __ _ _ __ ___   ___
      __) | \ \ /\ / / `_ \/ __| | `__|  / _` |/ _` | __/ _` | |_| `__/ _` | `_ ` _ \ / _ \
     / __/   \ V  V /| |_) \__ \ | |    | (_| | (_| | || (_| |  _| | | (_| | | | | | |  __/
    |_____|   \_/\_/ | .__/|___/ |_|     \__,_|\__,_|\__\__,_|_| |_|  \__,_|_| |_| |_|\___|
                     |_|
    */
    proc print data=sd1.want;
    run;quit;

    proc datasets lib=sd1 nolist mt=data mt=view nodetails;delete want; run;quit;

    %utl_submit_wps64x("
    libname sd1 'd:/sd1';
    proc r;
    export data=sd1.have r=have;
    submit;
    library(dplyr);
    want <- have |>
      filter(grepl('^J', NAME));
    want;
    endsubmit;
    import data=sd1.want r=want;
    ");

    proc print data=sd1.want;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  The WPS R System                                                                                                      */
    /*                                                                                                                        */
    /*       NAME SEX AGE HEIGHT WEIGHT                                                                                       */
    /*  1   James   M  12   57.3   83.0                                                                                       */
    /*  2    Jane   F  12   59.8   84.5                                                                                       */
    /*  3   Janet   F  15   62.5  112.5                                                                                       */
    /*  4 Jeffrey   M  13   62.5   84.0                                                                                       */
    /*  5    John   M  12   59.0   99.5                                                                                       */
    /*  6   Joyce   F  11   51.3   50.5                                                                                       */
    /*  7    Judy   F  14   64.3   90.0                                                                                       */
    /*                                                                                                                        */
    /*  The WPS System                                                                                                        */
    /*                                                                                                                        */
    /*   NAME       SEX    AGE    HEIGHT    WEIGHT                                                                            */
    /*                                                                                                                        */
    /*   James       M      12     57.3       83.0                                                                            */
    /*   Jane        F      12     59.8       84.5                                                                            */
    /*   Janet       F      15     62.5      112.5                                                                            */
    /*   Jeffrey     M      13     62.5       84.0                                                                            */
    /*   John        M      12     59.0       99.5                                                                            */
    /*   Joyce       F      11     51.3       50.5                                                                            */
    /*   Judy        F      14     64.3       90.0                                                                            */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    proc datasets lib=sd1 nolist mt=data mt=view nodetails;delete want; run;quit;
    proc datasets lib=work nolist mt=data mt=view nodetails;delete want; run;quit;

    %utl_submit_wps64x("
    libname sd1 'd:/sd1';
    proc python;
    export data=sd1.have python=have;
    submit;
    import pyreadstat;
    want=have[have.NAME.str.match('^J')];
    print(want);
    pyreadstat.write_xport(want,'d:\\xpt\\want.xpt',table_name='want',file_format_version=5
    ,column_labels=['NAME','SEX','AGE','HEIGHT','WEIGHT']);
    endsubmit;
    ");

    /*--- handles long variable names by using the label to rename the variables  ----*/

    libname xpt xport "d:/xpt/want.xpt";
    proc contents data=xpt._all_;
    run;quit;

    data want_py_long_names;
      %utl_rens(xpt.want) ;
      set want;
    run;quit;
    libname xpt clear;


    proc print data=want_py_long_names;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* The WPS PYTHON Procedure                                                                                               */
    /*                                                                                                                        */
    /*         NAME SEX   AGE  HEIGHT  WEIGHT                                                                                 */
    /* 5   James      M  12.0    57.3    83.0                                                                                 */
    /* 6   Jane       F  12.0    59.8    84.5                                                                                 */
    /* 7   Janet      F  15.0    62.5   112.5                                                                                 */
    /* 8   Jeffrey    M  13.0    62.5    84.0                                                                                 */
    /* 9   John       M  12.0    59.0    99.5                                                                                 */
    /* 10  Joyce      F  11.0    51.3    50.5                                                                                 */
    /* 11  Judy       F  14.0    64.3    90.0                                                                                 */
    /*                                                                                                                        */
    /* The WPS System                                                                                                         */
    /*                                                                                                                        */
    /*  NAME       SEX    AGE    HEIGHT    WEIGHT                                                                             */
    /*                                                                                                                        */
    /*  James       M      12     57.3       83.0                                                                             */
    /*  Jane        F      12     59.8       84.5                                                                             */
    /*  Janet       F      15     62.5      112.5                                                                             */
    /*  Jeffrey     M      13     62.5       84.0                                                                             */
    /*  John        M      12     59.0       99.5                                                                             */
    /*  Joyce       F      11     51.3       50.5                                                                             */
    /*  Judy        F      14     64.3       90.0                                                                             */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */

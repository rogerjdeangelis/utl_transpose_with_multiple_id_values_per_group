# utl_transpose_with_multiple_id_values_per_group
Transpose With multiple ID values per Group. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    Transpose With multiple ID values per Group

    Same result in WPS and SAS

    see
    https://tinyurl.com/ycgllsuj
    https://stackoverflow.com/questions/50843351/proc-transpose-with-multiple-id-values-per-group


    INPUT
    =====
                                                |   RULES Hust add a numeric sufix to REPORTSTOTYPE
       EMPLOYEE_                                |   and transpose (see process)
           ID       REPORTSTO    REPORTSTOTYPE  |  REPORTSTOTYPE
                                                |
          100       Jane          Supervisor    |   Supervisor1
          100       Mark          Team_lead     |   Team_lead2
          100       Jamie         Team_lead     |   Team_lead3
                                                |
          101       Max           Supervisor    |   Supervisor1
          101       Marie         Team_lead     |   Team_lead2
          101       Satyendr      Team_lead     |   Team_lead3
          101       Usha          Team_lead     |   Team_lead4
          102       Sarah         Supervisor    |   Supervisor1
          102       Sam           Team_lead     |   Team_lead 2
        ...

    EXAMPLE OUTPUT

     WORK.HAVXPO total obs=3

             EMPLOYEE_                  TEAM_    TEAM_       TEAM_
      Obs        ID      SUPERVISOR1    LEAD2    LEAD3       LEAD4

       1        100         Jane        Mark     Jamie
       2        101         Max         Marie    Satyendr    Usha
       3        102         Sarah       Sam


    PROCESS
    =======

    * add sufix number;
    data havSeq;
     retain cnt 0;
     set have;
     by employee_id;
     cnt+1;
     ReportsToType = cats(ReportsToType,put(cnt,2.));
     if last.employee_id then cnt=0;
    run;quit;

    * simple transpose;
    proc transpose data=havSeq out=havXpo;
      by employee_id;
      id ReportsToType;
      var ReportsTo;
    run;quit;


    OUTPUT
    ======

     WORK.HAVXPO total obs=3

             EMPLOYEE_                  TEAM_    TEAM_       TEAM_
      Obs        ID      SUPERVISOR1    LEAD2    LEAD3       LEAD4

       1        100         Jane        Mark     Jamie
       2        101         Max         Marie    Satyendr    Usha
       3        102         Sarah       Sam

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    data have;
     input employee_id ReportsTo $ ReportsToType $12.;
    cards4;
    100 Jane Supervisor
    100 Mark Team_lead
    100 Jamie Team_lead
    101 Max Supervisor
    101 Marie Team_lead
    101 Satyendra Team_lead
    101 Usha Team_lead
    102 Sarah Supervisor
    102 Sam Team_lead
    ;;;;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    %utl_submit_wps64('
    libname wrk sas7bdat "%sysfunc(pathname(work))";
    data havSeq;
     retain cnt 0;
     set wrk.have;
     by employee_id;
     cnt+1;
     ReportsToType = cats(ReportsToType,put(cnt,2.));
     if last.employee_id then cnt=0;
    run;quit;

    * simple transpose;
    proc transpose data=havSeq out=havXpo;
      by employee_id;
      id ReportsToType;
      var ReportsTo;
    run;quit;
    run;quit;

    proc print;
    run;quit;
    ');


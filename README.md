# utl_indentify_round_trips
Identify Round Trips.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    Identify Round Trips

    github
    https://github.com/rogerjdeangelis/utl_indentify_round_trips

    see
    https://communities.sas.com/t5/Base-SAS-Programming/SAS-Scenario/m-p/446760


    INPUT
    =====

      WORK.HAVE total obs=5

        ID    SOURCE      DESTINATION

         1    Delhi        Bangalore
         2    Delhi        Goa
         1    Bangalor     Goa
         2    Goa          Bangalore
         1    Goa          Delhi

       RULES ( first sort by id with equals option to maintain destination order)

        ID    SOURCE      DESTINATION

         1    Delhi        Bangalore
         1    Bangalor     Goa
         1    Goa          Delhi         Round trip because first source= last destination



    PROCESS ( All the code )
    ========================

    proc sort data=have out=havSrt equals;  * keep order;
      by id;
    run;quit;

    data want;
     length home $16;
     retain home;
     set havSrt;
     by id;
     * retain home=source;
     if first.id then home=source;
     * if last check destination;
      if last.id then do;
        if  destination=home then roundtrip='Yes';
        else roundtrip='No';
        keep roundtrip home destination;
        output;
     end;
    run;quit;


    OUTPUT
    ======

     WORK.WANT total obs=2

        HOME     DESTINATION    ROUNDTRIP

        Delhi     Delhi            Yes
        Delhi     Bangalore        No

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    Data have;
    input id source $ destination & $16.;
    cards4;
    1 Delhi Bangalore
    2 Delhi Goa
    1 Bangalore Goa
    2 Goa Bangalore
    1 Goa Delhi
    ;;;;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;
    proc sort data=have out=havSrt equals;  * keep order;
      by id;
    run;quit;

    data want;
     length home $16;
     retain home;
     set havSrt;
     by id;
     if first.id then home=source;
     if last.id then do;
      if  destination=home then roundtrip='Yes';
      else roundtrip='No';
      keep roundtrip home destination;
      output;
     end;
    run;quit;



    proc sort data=have out=havSrt equals;  * keep order;
      by id;
    run;quit;

    data want;
     length home $16;
     retain home;
     set havSrt;
     by id;
     if first.id then home=source;
     if last.id then do;
      if  destination=home then roundtrip='Yes';
      else roundtrip='No';
      keep roundtrip home destination;
      output;
     end;
    run;quit;


    Up to 40 obs WORK.WANT total obs=2

    Obs    HOME     DESTINATION    ROUNDTRIP

     1     Delhi     Delhi            Yes
     2     Delhi     Bangalore        No


    * wps;
    Data have;
    input id source $ destination & $16.;
    cards4;
    1 Delhi Bangalore
    2 Delhi Goa
    1 Bangalore Goa
    2 Goa Bangalore
    1 Goa Delhi
    ;;;;
    run;quit;


    %utl_submit_wps64('
    libname wrk sas7bdat "%sysfunc(pathname(work))";

    proc sort data=wrk.have out=havSrt equals;
      by id;
    run;quit;

    data wrk.wantwps;
     length home $16;
     retain home;
     set havSrt;
     by id;
     if first.id then home=source;
     if last.id then do;
      if  destination=home then roundtrip="Yes";
      else roundtrip="No";
      keep roundtrip home destination;
      output;
     end;
    run;quit;
    ');


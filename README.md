# utl_converting_32bit_ms_access_tables_to_sas_datasets_without_sas_access_products
Converting 32bit MS access tables to SAS datasets without SAS Access products.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    Converting 32bit MS access tables to SAS datasets without SAS Access products

    github
    https://tinyurl.com/yanqfy8o
    https://github.com/rogerjdeangelis/utl_converting_32bit_ms_access_tables_to_sas_datasets_without_sas_access_products

    Note that the "bitness" (64-bit or 32-bit) of the Access ODBC driver must match
    the version of R you are running. That is, if you are running 32-bit R then you
    need the 32-bit drivers even though you are running 64-bit Windows.

    INPUT (32bit MS Access Table class)
    ===================================

      d:/mdb/class.mdb
      with table class

     EXAMPLE OUTPUT SAS DATSETS

     WORK.CLASSFROMR total obs=19

       NAME       SEX    AGE    HEIGHT    WEIGHT

       Alfred      M      14     69.0      112.5
       Alice       F      13     56.5       84.0
       Barbara     F      13     65.3       98.0
       Carol       F      14     62.8      102.5
     ....


    SOLUTION
    ========

    proc datasets lib=work kill;
    run;quit;

    * need 32bit R;
    %utl_submit_r32("
        library(RODBC);
        myDB <- odbcConnectAccess('d:/mdb/class.mdb',uid='admin',pwd='');
        class<-sqlQuery(myDB,'select * from class');
        odbcCloseAll();
        save.image('d:/rda/class.Rdata');
    ");

    * if you happen to have IML;
    * convert dataframe males to a SAS dataset;
    proc iml;
      submit / R;
        load('d:/rda/class.Rdata');
      endsubmit;
      run importdatasetfromr("work.classFromR","class");
    run;quit;

    Proc print data=classFromR;
    run;quit;

    LOG

    > library(RODBC);
    myDB <- odbcConnectAccess('d:/mdb/class.mdb',uid='admin',pwd='');
    class<-sqlQuery(myDB,'select * from class');
    odbcCloseAll();    save.image('d:/rda/class.Rdata');
    >
    NOTE: 3 lines were written to file PRINT.
    Stderr output:
    Warning message:
    package 'RODBC' was built under R version 3.3.3
    NOTE: 2 records were read from the infile RUT.
          The minimum record length was 2.
          The maximum record length was 190.
    NOTE: DATA statement used (Total process time):
          real time           3.11 seconds
          user cpu time       0.01 seconds
          system cpu time     0.04 seconds
          memory              260.65k
          OS Memory           15852.00k
          Timestamp           08/10/2018 11:48:09 AM
          Step Count                        220  Switch Count  0


    MPRINT(UTL_SUBMIT_R32):   filename rut clear;
    NOTE: Fileref RUT has been deassigned.
    MPRINT(UTL_SUBMIT_R32):   filename r_pgm clear;
    NOTE: Fileref R_PGM has been deassigned.
    MLOGIC(UTL_SUBMIT_R32):  Ending execution.
    710   * if you happen to have IML;
    711   * convert dataframe males to a SAS dataset;
    712   proc iml;
    NOTE: IML Ready
    713     submit / R;
    714       load('d:/rda/class.Rdata');
    715     endsubmit;
    716     run importdatasetfromr("work.classFromR","class");
    717   run;
    NOTE: Module MAIN is undefined in IML; cannot be RUN.
    717 !     quit;
    NOTE: Exiting IML.
    NOTE: PROCEDURE IML used (Total process time):
          real time           0.38 seconds
          user cpu time       0.01 seconds
          system cpu time     0.01 seconds
          memory              484.09k
          OS Memory           15852.00k
          Timestamp           08/10/2018 11:48:09 AM
          Step Count                        221  Switch Count  6


    718   Proc print data=classFromR;
    719   run;

    NOTE: There were 19 observations read from the data set WORK.CLASSFROMR.
    NOTE: PROCEDURE PRINT used (Total process time):
          real time           0.03 seconds
          user cpu time       0.01 seconds
          system cpu time     0.01 seconds
          memory              327.28k
          OS Memory           15852.00k
          Timestamp           08/10/2018 11:48:09 AM
          Step Count                        222  Switch Count  0


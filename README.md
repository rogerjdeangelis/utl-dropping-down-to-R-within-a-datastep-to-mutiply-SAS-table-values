# utl-dropping-down-to-R-within-a-datastep-to-mutiply-SAS-table-values
Dropping down to R within a datastep to mutiply SAS table values

    Dropping down to R within a datastep to mutiply SAS table values


    github
    https://tinyurl.com/yyvshuwu
    https://github.com/rogerjdeangelis/utl-dropping-down-to-R-within-a-datastep-to-mutiply-SAS-table-values

    https://tinyurl.com/y65uss8w
    https://communities.sas.com/t5/SAS-Programming/Multiply-vector-data-with-base-sas-a-vector-b-dataset-in-python/m-p/560176

    *_                   _
    (_)_ __  _ __  _   _| |_
    | | '_ \| '_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    ;

    %let mat=  1, 4,
               2, 3,
               3, 2 ;

    %let vec=  2,
               3  ;

    *           _
     _ __ _   _| | ___  ___
    | '__| | | | |/ _ \/ __|
    | |  | |_| | |  __/\__ \
    |_|   \__,_|_|\___||___/

    ;
      t(mat) * vec        WANT
                          ----
            1       4     2  12
        2 * 2   3 * 3  =  4   9
            3       2     6   6

    *            _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| '_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    ;

    WORK.WANT total obs=3

      V1    V2

       2    12
       4     9
       6     6

    *
     _ __  _ __ ___   ___ ___  ___ ___
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    ;

    proc datasets lib=work;
      delete want;
    run;quit;

    %utlfkil(d:/xpt/want.xpt);

    %utl_submit_r64("
       library(haven);
       library(SASxport);
       library(data.table);
       mat <- matrix(c(&mat), nrow = 3, ncol=2, byrow=TRUE);
       vec <- c(&vec);
       want<-t(mat) * vec;
       want<-as.data.table(t(want));
       write.xport(want,file='d:/xpt/want.xpt');
    ");

    libname xpt xport "d:/xpt/want.xpt";
    data want;
      set xpt.want;
    run;quit;
    libname xpt clear;



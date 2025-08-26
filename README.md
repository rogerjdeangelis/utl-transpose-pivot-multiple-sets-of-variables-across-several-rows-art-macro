# utl-transpose-pivot-multiple-sets-of-variables-across-several-rows-art-macro
Transpose pivot multiple sets of variables across several rows art macro
    %let pgm=utl-transpose-pivot-multiple-sets-of-variables-across-several-rows-art-macro;

    %stop_submission;

    Transpose pivot multiple sets of variables across several rows art macro

      TWO SOLUTIONS

          1 order val1_a val2_a val3_a
          2 order val1_a val1_b val1_c

    https://tinyurl.com/yunsfavs
    https://github.com/rogerjdeangelis/utl-transpose-pivot-multiple-sets-of-variables-across-several-rows-art-macro/new/main

    github
    sas communities
    https://tinyurl.com/3majw644
    https://communities.sas.com/t5/SAS-Programming/How-to-transpose-from-same-row-with-multiple-variables/m-p/758577#M239547

    /**************************************************************************************************************************/
    /* INPUT                       | PROCESS                                  | PROCESS                                       */
    /* =====                       | =======                                  | =======                                       */
    /* WORK.HAVE                   | 1 ORDER VAL1_A VAL2_A VAL3_A             |                                               */
    /*  ID SOLD VAL1 VAL2 VAL3     | ============================             |       V   V  V    V   V V    V   V  V         */
    /*                             |                                          |       A   A  A    A   A A    A   A  A         */
    /*  F1  A    13   207   1      | %utl_transpose(                          |       L   L  L    L   L L    L   L  L         */
    /*  F1  A    13   207   1      |     data=have                            |       1   2  3    1   2 3    1   2  3         */
    /*  F1  A    13   207   1      |    ,out=want                             |   I   -   -  -    -   - -    -   -  -         */
    /*  F1  B    18   205   3      |    ,by=id                                |   D   A   A  A    B   B B    C   C  C         */
    /*  F1  B    18   205   3      |    ,id=sold                              |                                               */
    /*  F1  B    18   205   3      |    ,var=val1-val3                        |  F1  13 207  1   18 205 3   15 208  4         */
    /*  F1  B    18   205   3      |    ,delimiter=_);                        |  F2  19 220  8   14 210 6   12 212  5         */
    /*  F1  C    15   208   4      |                                          |  F3  15 230 10    .   . .    .   .  .         */
    /*  F1  C    15   208   4      | proc print data=want                     |                                               */
    /*  F2  A    19   220   8      |    heading=vertical                      |                                               */
    /*  F2  A    19   220   8      |    width=min;                            |                                               */
    /*  F2  B    14   210   6      | run;quit;                                |                                               */
    /*  F2  C    12   212   5      |                                          |                                               */
    /*  F3  A    15   230  10      |------------------------------------------------------------------------------------------*/
    /*                             |                                          |                                               */
    /* data have;                  | 2 VAL1_A VAL1_B VAL1_C                   |      V  V  V      V   V   V     V  V V        */
    /*   input ID $                | ======================                   |      A  A  A      A   A   A     A  A A        */
    /*   sold $                    |                                          |      L  L  L      L   L   L     L  L L        */
    /*   Val1-Val3;                | * SAME CODE;                             |      1  1  1      2   2   2     3  3 3        */
    /* cards4;                     | %utl_transpose(                          |  I   _  _  _      _   _   _     _  _ _        */
    /* F1 A 13 207 1               |     data=have                            |  D   A  B  C      A   B   C     A  B C        */
    /* F1 A 13 207 1               |    ,out=want                             |                                               */
    /* F1 A 13 207 1               |    ,by=id                                |  F1  13 18 15   207 205 208     1 3 4         */
    /* F1 B 18 205 3               |    ,id=sold                              |  F2  19 14 12   220 210 212     8 6 5         */
    /* F1 B 18 205 3               |    ,var=val1-val3                        |  F3  15  .  .   230   .   .    10 . .         */
    /* F1 B 18 205 3               |    ,delimiter=_);                        |                                               */
    /* F1 B 18 205 3               |                                          |                                               */
    /* F1 C 15 208 4               | %array(_vs,values=1-3);                  |                                               */
    /* F1 C 15 208 4               | data wantodr/view=wantodr;               |                                               */
    /* F2 A 19 220 8               |   retain id %do_over(_vs                 |                                               */
    /* F2 A 19 220 8               |    ,phrase=val?_a val?_b val?_c);        |                                               */
    /* F2 B 14 210 6               |   set want ;                             |                                               */
    /* F2 C 12 212 5               | run;                                     |                                               */
    /* F3 A 15 230 10              |                                          |                                               */
    /* ;;;;                        | proc print data=wantodr                  |                                               */
    /* run;quit;                   |    heading=vertical                      |                                               */
    /*                             |    width=min;                            |                                               */
    /*                             | run;quit;                                |                                               */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    data have;
      input ID $
      sold $
      Val1-Val3;
    cards4;
    F1 A 13 207 1
    F1 A 13 207 1
    F1 A 13 207 1
    F1 B 18 205 3
    F1 B 18 205 3
    F1 B 18 205 3
    F1 B 18 205 3
    F1 C 15 208 4
    F1 C 15 208 4
    F2 A 19 220 8
    F2 A 19 220 8
    F2 B 14 210 6
    F2 C 12 212 5
    F3 A 15 230 10
    ;;;;
    run;quit;



    /**************************************************************************************************************************/
    /*  ID    SOLD    VAL1    VAL2    VAL3                                                                                    */
    /*                                                                                                                        */
    /*  F1     A       13      207      1                                                                                     */
    /*  F1     A       13      207      1                                                                                     */
    /*  F1     A       13      207      1                                                                                     */
    /*  F1     B       18      205      3                                                                                     */
    /*  F1     B       18      205      3                                                                                     */
    /*  F1     B       18      205      3                                                                                     */
    /*  F1     B       18      205      3                                                                                     */
    /*  F1     C       15      208      4                                                                                     */
    /*  F1     C       15      208      4                                                                                     */
    /*  F2     A       19      220      8                                                                                     */
    /*  F2     A       19      220      8                                                                                     */
    /*  F2     B       14      210      6                                                                                     */
    /*  F2     C       12      212      5                                                                                     */
    /*  F3     A       15      230     10                                                                                     */
    /**************************************************************************************************************************/


    /*                 _                        _ _                     _ ____                       _ _____
    / |   ___  _ __ __| | ___ _ __  __   ____ _| / |   __ _ __   ____ _| |___ \    __ _  __   ____ _| |___ /    __ _
    | |  / _ \| `__/ _` |/ _ \ `__| \ \ / / _` | | |  / _` |\ \ / / _` | | __) |  / _` | \ \ / / _` | | |_ \   / _` |
    | | | (_) | | | (_| |  __/ |     \ V / (_| | | | | (_| | \ V / (_| | |/ __/  | (_| |  \ V / (_| | |___) | | (_| |
    |_|  \___/|_|  \__,_|\___|_|      \_/ \__,_|_|_|__\__,_|  \_/ \__,_|_|_____|__\__,_|   \_/ \__,_|_|____/___\__,_|
                                                  |____|                      |____|                      |_____|
    */

    %utl_transpose(
        data=have
       ,out=want
       ,by=id
       ,id=sold
       ,var=val1-val3
       ,delimiter=_);

    proc print data=want
       width=min;
    run;quit;

    /**************************************************************************************************************************/
    /*|  ID    VAL1_A    VAL2_A    VAL3_A    VAL1_B    VAL2_B    VAL3_B    VAL1_C    VAL2_C    VAL3_C                         */
    /*|                                                                                                                       */
    /*|  F1      13       207         1        18       205        3         15       208        4                            */
    /*|  F2      19       220         8        14       210        6         12       212        5                            */
    /*|  F3      15       230        10         .         .        .          .         .        .                            */
    /**************************************************************************************************************************/

    /*___                  _                        _ _                     _ _    _                  _ _
    |___ \    ___  _ __ __| | ___ _ __  __   ____ _| / |   __ _ __   ____ _| / |  | |__   __   ____ _| / |   ___
      __) |  / _ \| `__/ _` |/ _ \ `__| \ \ / / _` | | |  / _` |\ \ / / _` | | |  | `_ \  \ \ / / _` | | |  / __|
     / __/  | (_) | | | (_| |  __/ |     \ V / (_| | | | | (_| | \ V / (_| | | |  | |_) |  \ V / (_| | | | | (__
    |_____|  \___/|_|  \__,_|\___|_|      \_/ \__,_|_|_|__\__,_|  \_/ \__,_|_|_|__|_.__/    \_/ \__,_|_|_|__\___|
                                                      |____|                  |____|                    |____|
    */

    * SAME CODE;
    %utl_transpose(
        data=have
       ,out=want
       ,by=id
       ,id=sold
       ,var=val1-val3
       ,delimiter=_);

    %array(_vs,values=1-3);
    data wantodr/view=wantodr;
      retain id %do_over(_vs
       ,phrase=val?_a val?_b val?_c);
      set want ;
    run;

    proc print data=wantodr
       width=min;
    run;quit;

    /**************************************************************************************************************************/
    /*|  ID    VAL1_A    VAL1_B    VAL1_C    VAL2_A    VAL2_B    VAL2_C    VAL3_A    VAL3_B    VAL3_C                         */
    /*|                                                                                                                       */
    /*|  F1      13        18        15       207       205       208         1        3         4                            */
    /*|  F2      19        14        12       220       210       212         8        6         5                            */
    /*|  F3      15         .         .       230         .         .        10        .         .                            */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */

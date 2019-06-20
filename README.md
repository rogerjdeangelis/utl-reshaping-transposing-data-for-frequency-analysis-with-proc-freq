# utl-reshaping-transposing-data-for-frequency-analysis-with-proc-freq
Reshaping transposing data for frequency analysis with proc freq
    Reshaping transposing data for frequency analysis with proc freq                                                   
                                                                                                                       
    This is a fairly common problem                                                                                    
                                                                                                                       
      Four solutions                                                                                                   
                                                                                                                       
           1. Proc transpose and a datastep                                                                            
           2. Single 'proc sql' using sql arrays                                                                       
           3. Single datastep                                                                                          
           4. True single datastep(HASH) - proc freq not needed (by Paul Dorfman)                                      
              Paul Dorfman                                                                                             
              sashole@bellsouth.net                                                                                    
                                                                                                                       
    github                                                                                                             
    https://tinyurl.com/y5e5kjbv                                                                                       
    https://github.com/rogerjdeangelis/utl-reshaping-transposing-data-for-frequency-analysis-with-proc-freq            
                                                                                                                       
    SAS Forum                                                                                                          
    https://tinyurl.com/y2jsejpu                                                                                       
    https://communities.sas.com/t5/New-SAS-User/proc-transpose-vs-proc-sql/m-p/566807                                  
                                                                                                                       
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
                                                                                                                       
    data have;                                                                                                         
      input male_married female_married male_single female_single ;                                                    
    cards4;                                                                                                            
    7 2 5 3                                                                                                            
    ;;;;                                                                                                               
    run;quit;                                                                                                          
                                                                                                                       
    Up to 40 obs from HAVE total obs=1                                                                                 
                                                                                                                       
            MALE_     FEMALE_     MALE_    FEMALE_                                                                     
    Obs    MARRIED    MARRIED    SINGLE     SINGLE                                                                     
                                                                                                                       
     1        7          2          5         3                                                                        
                                                                                                                       
                                                                                                                       
    *            _               _                                                                                     
      ___  _   _| |_ _ __  _   _| |_ ___                                                                               
     / _ \| | | | __| '_ \| | | | __/ __|                                                                              
    | (_) | |_| | |_| |_) | |_| | |_\__ \                                                                              
     \___/ \__,_|\__| .__/ \__,_|\__|___/                                                                              
                    |_|                                                                                                
    ;                                                                                                                  
                                                                                                                       
    Up to 40 obs WORK.HAVFRQ total obs=4                                                                               
                                                                                                                       
                     MARITAL_                                                                                          
    Obs     SEX       STATUS     WGT                                                                                   
                                                                                                                       
     1     MALE      MARRIED      7                                                                                    
     2     FEMALE    MARRIED      2                                                                                    
     3     MALE      SINGLE       5                                                                                    
     4     FEMALE    SINGLE       3                                                                                    
                                                                                                                       
                                                                                                                       
    Up to 40 obs from WANT total obs=4                                                                                 
                                                                                                                       
           MARITAL_                                                                                                    
    Obs     STATUS      SEX      COUNT    PERCENT                                                                      
                                                                                                                       
     1     MARRIED     FEMALE      2      11.7647                                                                      
     2     MARRIED     MALE        7      41.1765                                                                      
     3     SINGLE      FEMALE      3      17.6471                                                                      
     4     SINGLE      MALE        5      29.4118                                                                      
                                                                                                                       
    *                                                                                                                  
     _ __  _ __ ___   ___ ___  ___ ___  ___  ___                                                                       
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|/ _ \/ __|                                                                      
    | |_) | | | (_) | (_|  __/\__ \__ \  __/\__ \                                                                      
    | .__/|_|  \___/ \___\___||___/___/\___||___/                                                                      
    |_|                                                                                                                
    ;                                                                                                                  
                                                                                                                       
    *_       _____                                                                                                     
    / |     |_   _| __ __ _ _ __  ___ _ __   ___  ___  ___                                                             
    | |       | || '__/ _` | '_ \/ __| '_ \ / _ \/ __|/ _ \                                                            
    | |_      | || | | (_| | | | \__ \ |_) | (_) \__ \  __/                                                            
    |_(_)     |_||_|  \__,_|_| |_|___/ .__/ \___/|___/\___|                                                            
                                     |_|                                                                               
    ;                                                                                                                  
                                                                                                                       
    data have;                                                                                                         
      input male_married female_married male_single female_single ;                                                    
    cards4;                                                                                                            
    7 2 5 3                                                                                                            
    ;;;;                                                                                                               
    run;quit;                                                                                                          
                                                                                                                       
    proc transpose data=have out=havXpo;                                                                               
    run;quit;                                                                                                          
                                                                                                                       
    Up to 40 obs WORK.WANT total obs=4                                                                                 
                                                                                                                       
    Obs        _NAME_        COL1                                                                                      
                                                                                                                       
     1     MALE_MARRIED        7                                                                                       
     2     FEMALE_MARRIED      2                                                                                       
     3     MALE_SINGLE         5                                                                                       
     4     FEMALE_SINGLE       3                                                                                       
                                                                                                                       
    data havFrq;                                                                                                       
      set havXpo;                                                                                                      
      sex=scan(_name_,1,"_");                                                                                          
      marital_status=scan(_name_,2,"_");                                                                               
      wgt=col1;                                                                                                        
      drop _name_ col1;                                                                                                
    run;quit;                                                                                                          
                                                                                                                       
    proc freq data=havFrq;                                                                                             
     tables marital_status*sex/missing out=want;                                                                       
     weight wgt;                                                                                                       
    run;quit;                                                                                                          
                                                                                                                       
    *____                 _                                                                                            
    |___ \      ___  __ _| |    __ _ _ __ _ __ __ _ _   _                                                              
      __) |    / __|/ _` | |   / _` | '__| '__/ _` | | | |                                                             
     / __/ _   \__ \ (_| | |  | (_| | |  | | | (_| | |_| |                                                             
    |_____(_)  |___/\__, |_|   \__,_|_|  |_|  \__,_|\__, |                                                             
                       |_|                          |___/                                                              
    ;                                                                                                                  
                                                                                                                       
    %array(cmbs,values=male_married female_married male_single female_single);                                         
                                                                                                                       
    proc sql;                                                                                                          
       create                                                                                                          
          table sqlXpo as                                                                                              
            %do_over(cmbs,phrase=%str(                                                                                 
            select                                                                                                     
                scan("?",1,"_")  as sex                                                                                
               ,scan("?",2,"_")  as marital_status                                                                     
               ,?  as wgt                                                                                              
            from                                                                                                       
               have)                                                                                                   
           ,between=union)                                                                                             
    ;quit;                                                                                                             
                                                                                                                       
    proc freq data=sqlXpo;                                                                                             
     tables marital_status*sex/missing out=want;                                                                       
     weight wgt;                                                                                                       
    run;quit;                                                                                                          
                                                                                                                       
    *_____         _       _                 _                                                                         
    |___ /      __| | __ _| |_ __ _ ___  ___| |_    __ _ _ __ _ __ __ _ _   _                                          
      |_ \     / _` |/ _` | __/ _` / __|/ _ \ __|  / _` | '__| '__/ _` | | | |                                         
     ___) |   | (_| | (_| | || (_| \__ \  __/ |_  | (_| | |  | | | (_| | |_| |                                         
    |____(_)   \__,_|\__,_|\__\__,_|___/\___|\__|  \__,_|_|  |_|  \__,_|\__, |                                         
                                                                        |___/                                          
    ;                                                                                                                  
                                                                                                                       
    data havStp;                                                                                                       
                                                                                                                       
      array ary[4] male_married female_married male_single female_single;                                              
                                                                                                                       
      set have;                                                                                                        
                                                                                                                       
      do i=1 to dim(ary);                                                                                              
         sex=scan(vname(ary[i]),1,"_");                                                                                
         marital_status=scan(vname(ary[i]),2,"_");                                                                     
         wgt=ary[i];                                                                                                   
         output;                                                                                                       
      end;                                                                                                             
                                                                                                                       
      drop male_married female_married male_single female_single;                                                      
                                                                                                                       
    run;quit;                                                                                                          
                                                                                                                       
    proc freq data=havStp;                                                                                             
     tables marital_status*sex/missing out=want;                                                                       
     weight wgt;                                                                                                       
    run;quit;                                                                                                          
                                                                                                                       
                                                                                                                       
    *_  _       _               _                                                                                      
    | || |     | |__   __ _ ___| |__                                                                                   
    | || |_    | '_ \ / _` / __| '_ \                                                                                  
    |__   _|   | | | | (_| \__ \ | | |                                                                                 
       |_|(_)  |_| |_|\__,_|___/_| |_|                                                                                 
                                                                                                                       
    ;                                                                                                                  
                                                                                                                       
    data have ;                                                                                                        
      input male_married female_married male_single female_single ;                                                    
      cards ;                                                                                                          
    7 2 5 3                                                                                                            
    1 4 6 8                                                                                                            
    9 3 2 1                                                                                                            
    run;                                                                                                               
                                                                                                                       
    data want (keep = marital_status sex count percent) ;                                                              
      dcl hash h (ordered:"A") ;                                                                                       
      h.definekey  ("marital_status", "sex") ;                                                                         
      h.definedata ("marital_status", "sex", "count") ;                                                                
      h.definedone () ;                                                                                                
      do until (z) ;                                                                                                   
        set have end = z ;                                                                                             
        array vn _numeric_ ;                                                                                           
        length marital_status sex $ 8 ;                                                                                
        do over vn ;                                                                                                   
          sex            = scan (vname (vn), 1, "_") ;                                                                 
          marital_status = scan (vname (vn), 2, "_") ;                                                                 
          if h.find() ne 0 then COUNT = vn ;                                                                           
          else                  COUNT + vn ;                                                                           
          h.replace() ;                                                                                                
        end ;                                                                                                          
        tcount + sum (of vn[*]) ;                                                                                      
      end ;                                                                                                            
      dcl hiter hi ("h") ;                                                                                             
      do while (hi.next() = 0) ;                                                                                       
        PERCENT = 100 * divide (COUNT, tcount) ;                                                                       
        output ;                                                                                                       
      end ;                                                                                                            
    run ;                                                                                                              
                                                                                                                       
    Best regards                                                                                                       
                                                                                                                       
                                                                                           

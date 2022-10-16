# SAS
/*1.a*/
libname hw '/home/u58879515/HW';
proc format;	
	value sizefmt
	low-<2='Less than 2.0 L'
	2-4  ='Between 2.0 and 4.0 L'
	4<-high='More than 4.0 L';
	
ods listing file='/home/u58879515/cars1.lst';	
proc report data=hw.cars;
title 'List Report of Vehicle Prices';
options pageno=1;
column MSRP Type EngineSize;
define Type / width=10 'Type of Vehicle';
define EngineSize / format=sizefmt. width=22;
define MSRP / format=dollar8.0 width=25 'Manufacture‘s 
										Suggested Retail Price';
run;
ods listing close;

/*1.b*/
ods listing file='/home/u58879515/cars2.lst';

proc report data=hw.cars headline;
title 'Summary Report of Vehicle Prices';
options pageno=1;
column MSRP Type EngineSize;
define Type / group width=10 'Type of Vehicle';
define EngineSize / group format=sizefmt. width=22;
define MSRP/ mean format=dollar8.0 width=25 'Manufacture‘s 
											Suggested Retail Price';
break after Type / summarize ol dul;
rbreak after / summarize dol;
run;

ods listing close;

/*2.a*/
libname data1 '/home/u58879515/data1';
proc print data=data1.laguardia;
run;

proc gchart data=data1.laguardia;
	pie Dest /type=freq fill=solid explode='LON';
run;

/*2.b*/
proc gchart data=data1.laguardia;
	hbar dest / sumvar=revenue type=mean;
	format revenue dollar12.2;
run;

/*2.c*/
proc gplot data=data1.laguardia;
plot revenue * boarded /  regeqn haxis=50 to 350 by 30;
symbol color=green v=star i=rlCLM98;
label Revenue='Flight Revenue'
	  boarded='Number of Passengers Boarded';
run;
quit;


/*3.a*/
proc print data=hw.military;
run;
Data Airforce(keep= type code state airport)
	 Army (drop=airport)
	 Marines(keep=code state) 
	 Naval;
	 set hw.military;
if Type='Air Force' then output Airforce;
else if Type='Army' then output Army;
else if Type='Marines' then output Marines;
else if Type='Naval' then output Naval;
run;

/*3.b*/
proc contents data=hw.military;
run;
proc contents data=airforce;
run;
proc contents data=army;
run;
proc contents data=marines;
run;
proc contents data=naval;
run;
/*There are 137 observations in Military data set.;
  There are 64 observations in  dataset Airforce.;
  There are 41 observations in  dataset Army;
  There are 4 observations in dataset Marine;
  There are 28 observations in dataset Naval;
*/
/*3.c*/
Data m1;
Set hw.military;
AwardsRT+Awards;
Run;
Proc print data=m1;
Run;

Data m2;
Set hw.military;
AwardsRT=sum(AwardsRT,Awards);
Retain;
Run;
Proc print data=m2;
Run;

/*3.d*/
Proc sort data=hw.military out=militarysorted;
By state;
Run;
data m3;
set hw.military;
by State;
if First.State then TotalAwards=0;
TotalAwards+Awards;
if Last.State; 
keep State TotalAwards;
run;
proc print data=m3;
run;


do age=18 to 25; 

     end;

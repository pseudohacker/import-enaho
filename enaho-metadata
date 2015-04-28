* IMPORTACIÓN DE BASES DE DATO ENAHO

* Rutas de trabajo 
global basepath 	  "D:\BDrepo\ENAHO\"
global bd_yr="2007"

* Bases originales
global bd_vivienda = "enaho01-${bd_yr}-100.dta"
miemb = "$bases_originales\ENAHO\\\\`ano'\\\enaho01-`ano'-200.dta"
loc base_edu   = "$bases_originales\ENAHO\\\\`ano'\\\enaho01a-`ano'-300.dta"
loc base_sal   = "$bases_originales\ENAHO\\\\`ano'\\\enaho01a-`ano'-400.dta"
loc base_ing   = "$bases_originales\ENAHO\\\\`ano'\\\enaho01a-`ano'-500.dta"

loc base_viv  = "$bases_originales\ENAHO\\`ano'\enaho01-`ano'-100.dta"
loc base_equi = "$bases_originales\ENAHO\\`ano'\enaho01-`ano'-612.dta"
loc base_sum  = "$bases_originales\ENAHO\\`ano'\sumaria-`ano'.dta"
loc base_prog = "$bases_originales\ENAHO\\`ano'\enaho01-`ano'-700.dta"

* Bases temporales
loc temp_miemb = "$bases_intermedias\Actualización 2013\Regresión Lineal\tempmiemb_`ano'.dta"
loc temp_edu   = "$bases_intermedias\Actualización 2013\Regresión Lineal\tempedu_`ano'.dta"
loc temp_sal   = "$bases_intermedias\Actualización 2013\Regresión Lineal\tempsal_`ano'.dta"
loc temp_ing   = "$bases_intermedias\Actualización 2013\Regresión Lineal\temping_`ano'.dta"

loc temp_viv   = "$bases_intermedias\Actualización 2013\Regresión Lineal\tempviv_`ano'.dta"
loc temp_equi  = "$bases_intermedias\Actualización 2013\Regresión Lineal\tempequi_`ano'.dta"
loc temp_sum   = "$bases_intermedias\Actualización 2013\Regresión Lineal\tempsum_`ano'.dta"
loc temp_prog  = "$bases_intermedias\Actualización 2013\Regresión Lineal\tempprog_`ano'.dta"
loc temp_sum2   = "$bases_intermedias\Actualización 2013\Regresión Lineal\tempsum2_`ano'.dta"

loc temp_indi  = "$bases_intermedias\Actualización 2013\Regresión Lineal\base_individual_`ano'.dta"
loc temp_nbi   = "$bases_intermedias\Actualización 2013\Regresión Lineal\nbis_`ano'.dta"

use "`base_sum'", clear
*===============================
* MODULOS A NIVEL DE INDIVIDUO
*===============================
*Luego se utilizarán los modulos a nivel de individual  (Modulo 2, 3, 4 y 5)
cap log close
*log using "$bases_intermedias\Log`ano'.log", replace

*------------------------------------
* MODULO 2: MIEMBROS DEL HOGAR
*-----------------------------------
// 103, 970 personas; OJO, para miembros del hogar:  SUMA P201 para P204=1 & (P203!=8,9)
*Se seleccionan las variables de localización y caracateristicas familiares

use conglome vivienda hogar codperso ubigeo dominio estrato p204 p203 p207 p209 p208a using "`base_miemb'", clear

/*consistencia // cuento con 30, 453 hogares que tienen resultado completo o incompleto
gen miembro= (p204==1 & (p203!=8 & p203!=9)) if !missing(p204, p203)-> definicion sumaria
gen id_hogar=conglome+vivienda+hogar
codebook id_hogar */ 

save "`temp_miemb'" , replace

*------------------------------------
* MODULO 3: EDUCACION
*-----------------------------------
*Se seleccionan las variables de localización y educacion // 112,307  -> Mayores de 3 años
use conglome vivienda hogar codperso ubigeo dominio estrato codinfor p307 p306 p300a p301b p301c p301a p302 p208a using "`base_edu'", clear
save "`temp_edu'" , replace

/*consistencia // cuento con 25,901 hogares que tienen resultado completo o incompleto
gen id_hogar=conglome+vivienda+hogar
codebook id_hogar */ 

*------------------------------------
* MODULO 4: SALUD
*-----------------------------------
*Se seleccionan las variables de localización y salud
// 117731

use conglome vivienda codperso hogar ubigeo dominio estrato codinfor p400n p400i p203 p207 p208a p209 p4191 p4192 p4193 p4194 p4195 p4196 p4197 p4198 using "`base_sal'", clear

if "`ano'"=="2010" | "`ano'"=="2011"{
recode p4198 (0=2)
recode p4197 (0=2)
recode p4196 (0=2)
recode p4195 (0=2)
recode p4194 (0=2)
recode p4193 (0=2)
recode p4192 (0=2)
recode p4191 (0=2)       
        }
		
save "`temp_sal'" , replace

/*consistencia // cuento con 25, 901 hogares que tienen resultado completo o incompleto
gen id_hogar=conglome+vivienda+hogar
codebook id_hogar */ 

*------------------------------------
* MODULO 5: EMPLEO
*-----------------------------------
*Se seleccionan las variables de localización y caracteristicas laborales
//87982
use conglome vivienda codperso hogar ubigeo dominio estrato codinfor p500n p500i p501 p502 p503 p504 p506 p507 ocu500 using "`base_ing'", clear
save "`temp_ing'" , replace
/*consistencia // cuento con 30, 453 hogares que tienen resultado completo o incompleto
gen id_hogar=conglome+vivienda+hogar
codebook id_hogar */ 

*===============================
* MERGE A NIVEL DE INDIVIDUO
*===============================
**Uniendo bases individuales y creando variables // Se usan las variables de localización para hacer el merge

use "`temp_miemb'", clear

merge 1:1  conglome vivienda hogar codperso ubigeo dominio estrato using "`temp_sal'" , keep(3) nogen // me quedo con 117,731 -> 6,307 (no son miembros del hogar o tiene missing)
merge 1:1  conglome vivienda hogar codperso ubigeo dominio estrato using "`temp_edu'",  nogen   // me quedo con 117,731 
merge 1:1  conglome vivienda hogar codperso ubigeo dominio estrato using "`temp_ing'", nogen 

*keep p701_01 p701_02 p701_03 p701_04 p701_05 p701_06 p701_07 p701_08 p701_09 p701_10 p701_11 p710_01 p710_02 p710_03 p710_04 p710_05 p710_06 p710_07 p710_08 p710_09 p710_10 p710_11 p710_12 p710_13

*P8  Parentesco con jefe del hogar
clonevar PTES01=p203
recode PTES01 (7=10) (10=11)
label define ptes 1 "jefe/jefa" 2 "esposo/esposa" 3 "hijo/hija" 4 "yerno/nuera" 5 "nieto" 6 "padres/suegros" 7 "Hermanos/hermanas"  8 "trabajador hogar" 9 "pensionista" 10 "otros parientes" 11 "otros no parientes"
label values PTES01 ptes    
label var PTES01 "PTES Parentesco"
 
 *p9  Núcleo familiar
 
 
 *p11 Sexo 
clonevar SEXO01=p207
label var SEXO01 "Sexo"


*p12 Estado civil
clonevar ESTA01=p209
label var ESTA01 "Estado Civil"
/*
*USAR SOLO 2007-2011
* Estado Civil
recode ESTA01 (6=1) (1=3) (5=4) (4=5) (3=6)
label define esta 1 "Soltero/a" 2 "Casado/a" 3 "Conviviente" 4 "Separado/a" 5 "Divorciado/a" 6 "Viudo/a"
label values ESTA01 esta
*/
*p13 Seguro de salud 

gen SEGU01=p4191==1
gen SEGU02=p4194==1
gen SEGU03=p4192==1|p4193==1|p4196==1|p4197==1
gen SEGU04=p4195==1
gen SEGU05=p4198==1
gen SEGU06=p4191==2 & p4192==2 & p4193==2 & p4194==2 & p4196==2 & p4197==2 & p4198==2

label var SEGU01 "Essalud"
label var SEGU02 "FFA-FNP"
label var SEGU03 "Privado"
label var SEGU04 "SIS"
label var SEGU05 "Otro"
label var SEGU06 "No tiene"



label define seguro 1 "Essalud" 2 "FFA-FNP" 3 "Privado" 4 "SIS" 5 "Otro" 6 "No tiene"

*p13a idioma
clonevar IDIOMA=p300a
recode IDIOMA (5 6 7=5) (8=6) 
label define idioma 1 "Quechua" 2 "Aymara" 3 "otra lengua nativa" 4 "Castellano" 5 "Otra lengua extranjera" 6 "Es Sordomudo"
label values  IDIOMA idioma

*p14  leer y eescribir
clonevar LEE01=p302
label var LEE01 "Sabe leer y escribir"

*p15 nivel educativo
clonevar NIV01=p301a
label var NIV01 "Nivel Educativo"
recode NIV01 (3 4 =3) (5 6 =4) (7 8= 5) (9 10=6) (11=7)
label define educa 1 "Sin Nivel" 2 "Inicial" 3 "Primaria" 4 "Secundaria" 5 "Sup. No Univ." 6 "Sup. Univ." 7 "Postgrado"
label values NIV01 educa

*p16: ultimo año o grado de estudios aprobados*
clonevar ULT01=p301b
label var ULT01 "Ultimo grado aprobado"
clonevar ULT02=p301c
label var ULT02 "Ultimo año aprobado"

*p17 ocupacion
gen ERA01=.
replace ERA01= 1 if p507==3|p507==4
replace ERA01= 2 if p507==2
replace ERA01= 3 if p507==1
replace ERA01= 4 if p507==6
replace ERA01= 5 if p507==5
replace ERA01= 6 if ocu500==2|ocu500==3
replace ERA01= 7 if p306==1 & ERA01==. & p208a<19 & p208a==.
replace ERA01= 8 if ocu500==4 & ERA==.
**FALTA JUBILADO Y PERSONA QUE SE DEDICA A QUEHACERES DEL HOGAR*
label define era 1 "Trabajador Dependiente" 2 "Trabajador Independiente" 3 "Empleador" 4 "Trabajador del Hogar" 5 "TFNR" 6 "Desempleado" 7 "Estudiante" 8 "Inactivo"
label values ERA01 era
label var ERA01 "Categoría de la ocupación"

*p18 Sector económico al que pertenece (No se puede replicar artesanal)
*Sectores
gen SECT=1 if p506 > 0 & p506 <120
replace SECT=2 if p506 > 119 & p506 < 200
replace SECT=3 if p506 ==200
replace SECT=4 if p506>499 & p506<600
replace SECT=5 if p506 > 999 & p506 < 1500
replace SECT=6 if p506 > 4999 & p506 < 5500
replace SECT=7 if p506 > 7499 & p506 < 7600 
replace SECT=8 if (p506 > 5500 & p506 < 9611) & SECT != 7
replace SECT=9 if !missing(p506) & missing(SECT)& !missing(ocu500)

label define sect 1 "Agricultura" 2 "Ganadería" 3 "Forestal" 4 "Pesca" 5 "Minería" 6 "Comercio" 7 "Estado" 8 "Servicios" 9 "Otros"
label values SECT sect
label var SECT "Sector al que pertenece"

clonevar EDAD= p208a
order conglome vivienda codperso hogar ubigeo dominio estrato codinfor  p208a EDAD p203 PTES01 p207 SEXO01 p209 ESTA01 p4191 p4192 p4193 p4194 p4195 p4196 p4197 p4198 SEGU* p300a IDIOMA p302 LEE01 p301a NIV01 p301b p301c ULT01 ULT02 p507 ocu500 ERA01 p506 SECT
keep conglome vivienda codperso hogar ubigeo dominio estrato codinfor  p208a EDAD p203 PTES01 p207 SEXO01 p209 ESTA01 p4191 p4192 p4193 p4194 p4195 p4196 p4197 p4198 SEGU* p300a IDIOMA p302 LEE01 p301a NIV01 p301b p301c ULT01 ULT02 p507 ocu500 ERA01 p506 SECT
cd "C:\Users\csolis\Documents\Base datos\ENAHO\Bases"
gen a_o=`ano'

save base_miembros_2007_,replace




append using base_miembros_2008
append using base_miembros_2009
append using base_miembros_2010
append using base_miembros_2011
append using base_miembros_2012
append using base_miembros_2013
append using base_miembros_2014

save base_miembros_total,replace
*/
*p19 NO EXISTE PREGUNTA DE DISCAPACIDAD*

*===============================
* MERGE A NIVEL DE VIVIENDA
*===============================

clear

global bases_originales 	  "C:\Users\csolis\Documents\Carpeta Ordenada 200415 César\Bases de Datos\Insumos"
global bases_intermedias 	  "C:\Users\csolis\Documents\Carpeta Ordenada 200415 César\Bases de Datos\Bases Intermedias\"

local ano="2014"

local base_viv  = "$bases_originales\ENAHO\\`ano'\enaho01-`ano'-100.dta"
local base_equi = "$bases_originales\ENAHO\\`ano'\enaho01-`ano'-612.dta"
local base_prog = "$bases_originales\ENAHO\\`ano'\enaho01-`ano'-700.dta"
loc base_sum  = "$bases_originales\ENAHO\\`ano'\sumaria-`ano'.dta"
use "`base_viv'", clear
/*
UNION CON BASE DE PROGRAMAS SOCIALES: Diferente en 2007-2011 (Es reportada a nivel de persona y no hogares)

*2008 a 2011*
use  conglome vivienda hogar ubigeo dominio estrato p702 p703 using "`base_prog'", clear 
duplicates drop conglome vivienda hogar p702 p703, force
gen x=1
reshape wide x, i(conglome vivienda hogar p702 ubigeo dominio estrato ) j(p703)
collapse (sum) x*, by(conglome vivienda hogar ubigeo dominio estrato)
save "$bases_originales\ENAHO\\`ano'progs",replace

*2007*
use  conglome vivienda hogar codperso ubigeo dominio estrato codperso p703 using "`base_prog'", clear 
duplicates drop conglome vivienda hogar codperso p703, force
gen x=1
reshape wide x, i(conglome vivienda hogar codperso ubigeo dominio estrato ) j(p703)
collapse (sum) x*, by(conglome vivienda hogar ubigeo dominio estrato)
save "$bases_originales\ENAHO\\`ano'progs",replace
*/
use conglome vivienda hogar p612n p612 ubigeo dominio estrato using "`base_equi'", clear
reshape wide p612, i(conglome vivienda hogar ubigeo dominio estrato) j(p612n)
merge 1:1  conglome vivienda hogar ubigeo dominio estrato using "`base_viv'",nogen keepusing(fecent result conglome vivienda hogar ubigeo dominio estrato p101 p102 p103 p103a p104 p104a p105a p110 p111a p1121 p1122 p1123 p1124 p1126 p1125 p1127 p1138 p113a  p1141 p1142 p1143 p1144 p1145 factor07 ubigeo dominio estrato codccpp nomccpp longitud latitud tipenc)
merge 1:1  conglome vivienda hogar ubigeo dominio estrato using "`base_sum'",nogen keepusing(mieperho gashog2d  g05hd  g05hd1  g05hd2  g05hd3  g05hd4  g05hd5  g05hd6  ig06hd  g07hd  ig08hd  sg23  sig24  sg25  sig26  gru11hd  gru12hd1  gru12hd2  gru13hd1  gru13hd2  gru13hd3  gru21hd gru22hd1  gru22hd2  gru23hd1  gru23hd2 gru23hd3  gru24hd  gru31hd  gru32hd1 gru32hd2  gru33hd1  gru33hd2  gru33hd3 gru34hd  gru41hd  gru42hd1  gru42hd2 gru43hd1  gru43hd2  gru43hd3  gru44hd gru51hd  gru52hd1  gru53hd1  gru53hd2 gru53hd3  gru54hd  gru61hd  gru62hd1 gru62hd2  gru63hd1  gru63hd2  gru63hd3 gru64hd  gru71hd  gru72hd1  gru72hd2 gru73hd1  gru73hd2  gru73hd3  gru74hd gru81hd  gru82hd1  gru82hd2  gru83hd1 gru83hd2  gru83hd3  gru84hd  gru14hd gru14hd1  gru14hd2  gru14hd3  gru14hd4 gru14hd5  sg42  sg421  sg422  sg423  sg42d sg42d1  sg42d2  sg42d3 linpe linea gashog1d pobreza)
*2007-2011
*merge 1:1  conglome vivienda hogar ubigeo dominio estrato using "$bases_originales\ENAHO\\`ano'progs",nogen 
*2012-2013
merge 1:1  conglome vivienda hogar ubigeo dominio estrato using "`base_prog'",nogen 

keep if result==1|result==2
           
		   
/*		   
*p20: SOLO 2007-2011: programas
gen PROG01=x1>0 & x1!=.
gen PROG02=x2>0 & x2!=.
gen PROG03=(x3>0 & x3!=.|x4>0 & x4!=.) 
gen PROG04=x5>0 & x5!=.
gen PROG05=x6>0 & x6!=.	
gen PROG10=x7>0 & x7!=. 
egen todos=rsum(x1 x2 x3 x4 x5 x6 x7)
gen PROG11=todos==0  
		   
label var PROG01 "Vaso de Leche"
label var PROG02 "Comedor Popular"
label var PROG03 "Desayuno o almuerzo escolar"
label var PROG04 "Papilla o Yapita"
label var PROG05 "Canasta Alimentaria"	
label var PROG10 "Otros"
label var PROG11 "Ninguno"	
*/   
*SOLO 2012-2013: p20 programas
/*
gen PROG01=p701_01==1
gen PROG02=p701_02==1
gen PROG03=p701_06==1|p701_07==1
*gen PROG04=1 if 
*gen PROG05=1 if 
gen PROG06=p710_03==1
gen PROG07=p710_11==1
gen PROG08=p710_05==1
gen PROG09=p710_01==1
gen PROG010=(p701_03==1|p701_04==1|p701_05==1|p701_08==1|p701_09==1|p701_10==1|p710_02==1|p710_04==1|p710_06==1|p710_07==1|p710_08==1|p710_09==1|p710_10==1)
gen PROG011=p710_11==1
label var PROG01 "Vaso de Leche"
label var PROG02 "Comedor Popular"
label var PROG03 "Desayuno o almuerzo escolar"
*label var PROG04 "Papilla o Yapita"
*label var PROG05 "Canasta Alimentaria"
label var PROG06 "Juntos"
label var PROG07 "Techo Propio o Mi Vivienda"
label var PROG08 "Pensión 65"
label var PROG09 "Cuna Más"
label var PROG010 "Otros"
label var PROG011 "Ninguno"
*/

*2014
gen PROG01=p701_01==1
gen PROG02=p701_02==1
gen PROG03=p701_03==1|p701_04==1
*gen PROG04=1 if 
*gen PROG05=1 if 
gen PROG06=p710_04==1
gen PROG07=.
gen PROG08=p710_05==1
gen PROG09=p710_01==1|p710_02==1
gen PROG10=(p710_06==1|p710_07==1|p710_08==1|p710_09==1|p710_10==1|p710_11==1)
gen PROG11=(p710_14==1|p701_09==1) & (PROG01==0 & PROG02==0 & PROG03==0 &  PROG06==0 & PROG08==0 & PROG09==0 & PROG10==0)
label var PROG01 "Vaso de Leche"
label var PROG02 "Comedor Popular"
label var PROG03 "Desayuno o almuerzo escolar"
*label var PROG04 "Papilla o Yapita"
*label var PROG05 "Canasta Alimentaria"
label var PROG06 "Juntos"
label var PROG07 "Techo Propio o Mi Vivienda"
label var PROG08 "Pensión 65"
label var PROG09 "Cuna Más"
label var PROG10 "Otros"
label var PROG11 "Ninguno"



label define progri 0 "No" 1 "Si"

label values PROG01 progri
label values PROG02 progri
label values PROG03 progri
*label values PROG04 progri
*label values PROG05 progri
label values PROG06 progri
label values PROG07 progri
label values PROG08 progri
label values PROG09 progri
label values PROG10 progri
label values PROG11 progri


*P.1 Tipo de Vivienda: p101							  
clonevar TVIV=p101

*P.2 Su vivienda es: p105a
gen VIVES=.
replace VIVES=1  if  p105a==1
replace VIVES=2  if  p105a==4
replace VIVES=3  if  p105a==2
replace VIVES=4  if  p105a==3
replace VIVES=5  if  p105a==5
replace VIVES=6  if  p105a==6
replace VIVES=7  if  p105a==7

label define vives 1 "Alquilada" 2 "Propia, pagándola a plazos" 3 "Propia, totalmente pagada" 4 "Propia por invasión" 5 "Cedida por el centro de trabajo" 6 "Cedida por otro hogar o institución" 7 "Otro"
label values VIVES vives

*P.3 Material Predominante Pared: p102

clonevar MPARED=p102
recode  MPARED (3 4=3) (5=4) (6=5) (7=6) (8=7) (9=8)
label define MPARED	8 "otro material" 7 "estera" 6 "madera" 5 "piedra con barro" 4 "quincha (caña con barro)" ///
					3 "adobe o tapia" 2 "piedra o sillar con cal o cemento" 1 "ladrillo o bloque de cemento"
label values MPARED MPARED


*p.4: Material Predominante Techos: p103a
clonevar MTECHO=p103a
label var MTECHO "Material del Piso"

*p5: Material Predominante en Pisos:p103
clonevar MPISO=p103
label var MPISO "Material del Piso"

*p6: Tipo de alumbrado

*Alumbrado //COMPATIBLE ENTRE ENAHOs: p1121-p1127

gen TALUM1=p1121==1
gen TALUM2=p1122==1
gen TALUM3=p1123==1
gen TALUM4=p1124==1
gen TALUM5=p1125==1 | p1126==1
gen TALUM6=p1127==1
label var TALUM1 "Electricidad"
label var TALUM2 "Kerosene"
label var TALUM3 "Petróleo/Gas"
label var TALUM4 "Vela"
label var TALUM5 "Otro"
label var TALUM6 "No Tiene"

label define alumb 0 "No" 1 "Si"
label values TALUM1 alumb
label values TALUM2 alumb
label values TALUM3 alumb
label values TALUM4 alumb
label values TALUM5 alumb
label values TALUM6 alumb


*p.7: Abastecimiento de agua p110
clonevar ABASAG=p110


*p.8: Serv Higiénico p1
clonevar SERHIG=p111a
*recode SERHIG (1=1) (2=2) (3=3) (4=4) (5=5) (6 7=6) //2011 a código 2010
recode SERHIG (1=1) (2=2) (3 5=4) (4=3) (6=5) (7 8=6) //2012 en adelante a código 2010

label define desague 	6 "no tiene" 5 "río, acequia o canal" 4 "pozo ciego o negro/letrina" 3 "pozo séptico" ///
						2 "red pública fuera de la vivienda pero dentro del edificio" 1 "red pública dentro de la vivienda" , modify
label values s desague

replace SERHIG=p111 if a_o<2012
*p9: Distancia no replicable


**Hogares**

*p1: Número de habitaciones
clonevar CTAHAB=p104
label var CTAHAB "Numero de habs"
*p2: Combustible
clonevar COMBUSA=p113a
***NO EXISTE BOSTA O ESTIERCOL***
recode COMBUSA (1=1) (2 3=2) (4=3) (5=4) (6=5) (7=7) (0=8)
label define combustible 8 "no cocina" 7 "otro" 5 "leña" 4 "carbón" 3 "kerosene" 2 "gas" 1 "electricidad"
label values COMBUSA combustible 
label var COMBUSA "Combustible para cocinar"

		   


*p3: Su hogar tiene* 
clonevar BIEN1=p6124
label var BIEN1 "equipo de sonido"
clonevar BIEN2=p6122
label var BIEN2 "televisor a color"
clonevar BIEN3=p6125
label var BIEN3 "DVD"
clonevar BIEN4=p6129
label var BIEN4 "licuadora"
clonevar BIEN5=p61212
label var BIEN5 "refrigeradora/congeladora"
clonevar BIEN6=p61210
label var BIEN6 "cocina a gas"
clonevar BIEN7=p1141
label var BIEN7 "telefono fijo"
clonevar BIEN8=p6128
label var BIEN8 "plancha eléctrica"
clonevar BIEN9=p61213
label var BIEN9 "lavadora"
clonevar BIEN10=p6127
label var BIEN10 "computadora"
clonevar BIEN11=p61214
label var BIEN11 "horno microoondas"
clonevar BIEN12=p1144
label var BIEN12 "internet"
clonevar BIEN13=p1143
label var BIEN13 "cable"
clonevar BIEN14=p1142
label var BIEN14 "celular"
gen BIEN15=1 if BIEN1==2 &  BIEN2==2 & BIEN3==2 & BIEN4==2 & BIEN5==2 & BIEN6==2 &  BIEN7==2 & BIEN8==2 & BIEN9==2 & BIEN10==2 & BIEN11==2 & BIEN12==2 &  BIEN13==2 & BIEN14==2 
label var BIEN15 "no tiene"


*p4: miembros del hogar
clonevar TOTAL=mieperho
label var TOTAL "miembros del hogar" 

*p612* BIEN*


egen alimentos_fuera=rsum( g05hd g05hd1 g05hd2 g05hd3 g05hd4 g05hd5 g05hd6 ig06hd  g07hd  ig08hd)
egen gru1_alimentos=rsum(sg23  sig24  sg25  sig26  gru11hd  gru12hd1  gru12hd2  gru13hd1  gru13hd2  gru13hd3 gru14hd gru14hd1  gru14hd2  gru14hd3  gru14hd4 gru14hd5)
egen gru2_vescal=rsum(gru21hd gru22hd1  gru22hd2  gru23hd1  gru23hd2 gru23hd3  gru24hd)
egen gru3_alqui=rsum(gru31hd  gru32hd1 gru32hd2  gru33hd1  gru33hd2  gru33hd3 gru34hd)
egen gru4_muebles=rsum(gru41hd  gru42hd1  gru42hd2 gru43hd1  gru43hd2  gru43hd3  gru44hd)
egen gru5_salud=rsum(gru51hd  gru52hd1  gru53hd1  gru53hd2 gru53hd3  gru54hd)
egen gru6_transcom=rsum(gru61hd  gru62hd1 gru62hd2  gru63hd1  gru63hd2  gru63hd3 gru64hd)
egen gru7_diver=rsum(gru71hd  gru72hd1  gru72hd2 gru73hd1  gru73hd2  gru73hd3  gru74hd)
egen gru8_otros=rsum(gru81hd  gru82hd1  gru82hd2  gru83hd1 gru83hd2  gru83hd3  gru84hd)
egen gasto_equipamiento=rsum(sg42  sg421  sg422  sg423  sg42d sg42d1  sg42d2  sg42d3)

egen gasto_total=rsum(alimentos_fuera gru1_alimentos gru2_vescal gru3_alqui gru4_muebles gru5_salud gru6_transcom gru7_diver gru8_otros gasto_equipamiento)

label var gasto_total "replica gashog2d"

egen gasto_bienes_libres=rsum(g05hd g05hd1 g05hd2 g05hd3 g05hd4 g05hd5 g05hd6 g07hd sg23 sg25 gru21hd gru11hd gru14hd gru31hd gru41hd gru51hd gru61hd gru71hd gru81hd sg42 sg421 sg422 sg423)
order conglome vivienda hogar ubigeo dominio estrato codccpp nomccpp longitud latitud factor07 p101 TVIV p105a VIVES p102 MPARED p103a MTECHO p103 MPISO p1121-p1127 TALUM* p110 ABASAG p111 SERHIG p104 CTAHAB p113a COMBUSA  	p114* mieperho TOTAL 				
*2012
*rename ccpp codccpp
cd "C:\Users\csolis\Documents\Base datos\ENAHO\Bases"
gen a_o=`ano'
save base_vivienda_2014,replace
clear

*Agregando fecent*
foreach x in 2014{
cd "C:\Users\csolis\Documents\Base datos\ENAHO\Bases"
use base_vivienda_`x', clear
cd "C:\Users\csolis\Documents\Base datos\ENAHO\\\`x'"
merge 1:1 conglome vivienda hogar codccpp using enaho01-`x'-100, keepusing(fecent) keep(3) nogen force
cd "C:\Users\csolis\Documents\Base datos\ENAHO\Bases"
save base_vivienda_`x',replace
clear
}
*Agregando Densidad Poblacional*

*Agregando fecent*
foreach x in  2014{
cd "C:\Users\csolis\Documents\Base datos\ENAHO\Bases"
use base_vivienda_`x', clear
cap destring mes, replace
destring conglome vivienda hogar ubigeo, replace
cd "C:\Users\csolis\Documents\Base datos\ENAHO\Bases\Densidad"
merge 1:1 conglome vivienda hogar codccpp using densi-`x'-100, keepusing(rastervalu) keep(3) nogen
cd "C:\Users\csolis\Documents\Base datos\ENAHO\Bases"
save base_vivienda_`x',replace
clear
}
use base_vivienda_2011, clear
gen latitud_original=latitud
destring latitud,replace force
save base_vivienda_2011,replace

use base_vivienda_2014,clear
destring conglome vivienda hogar mes,replace
tostring p700i p710i,replace
save base_vivienda_2014,replace


cd "C:\Users\csolis\Documents\Base datos\ENAHO\Bases"
use base_vivienda_2007, clear
append using base_vivienda_2008
append using base_vivienda_2009
append using base_vivienda_2010

append using base_vivienda_2011
append using base_vivienda_2012
append using base_vivienda_2013
append using base_vivienda_2014


replace PROG10=PROG010 if PROG10==.
replace PROG11=PROG011 if PROG11==.




save base_vivienda_total,replace


clonevar SERHIG=p111a
recode SERHIG (1=1) (2=2) (3 5=4) (4=3) (6=5) (7 8=6) 
gen s=.
replace s=p111 if a_o<2012
replace s=SERHIG if a_o>2011
label define desague 	6 "no tiene" 5 "río, acequia o canal" 4 "pozo ciego o negro/letrina" 3 "pozo séptico" ///
						2 "red pública fuera de la vivienda pero dentro del edificio" 1 "red pública dentro de la vivienda" , modify
label values s desague

e contents here

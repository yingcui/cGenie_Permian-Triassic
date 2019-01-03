################################################################
### readme.txt #################################################
################################################################

For:
'Catastrophic volcanic carbon emission during the Permian-Triassic Super Greenhouse'
Ying Cui, Andy Ridgwell, Jiuyuan Wang, Feihong Ye, Feifei Zhang + others

################################################################
2018/12/06 -- readme.txt file creation (Y.C.)
################################################################

Code used to run the model experiments is provided.
User and base configuration files used to run these experiments are also provided.

The intention is to replicate results. 

### code version ###############################################

The specific GitHub version is December 06, 2018

### model experiments -- spinups ################################

All experiments are run from:
$HOME/cgenie.muffin/genie-main

The commands to run the spinups are listed as follows:

(1a) INITIAL SPINUP

The initial, 1st-stage closed system spin-up (see Cui et al. [2013]):
./runmuffin.sh cgenie.eb_go_gs_ac_bg_sg_rg_gl.p0251b.BASESCH4_YC YC p0251b.PO4.DIC13CDoubleInversionCui.SPIN1 20 (fatal error, cannot stat "fort.2": No such file or directory;) (Now on Jan-02-2019, this model starts to work after changing the forcing file to "pyyyyz.RpCO2_Rp13CO2_FRALK_FDIC_F13DIC_FCa")
./runmuffin.sh cgenie.eb_go_gs_ac_bg_sg_rg_gl.p0251b.BASESCH4 YC p0251b.PO4.DIC13CDoubleInversionCui.SPIN1 20 (fatal error, foram_p_13C is not defined)
./runmuffin.sh cgenie.eb_go_gs_ac_bg.p0251b.BASESFe YC p0251b.PO4.DIC13CDoubleInversionCui.SPIN1 20 (successfully tried on 01-02-2019)
./runmuffin.sh cgenie.eb_go_gs_ac_bg.p0251b.BASESFeTDTL / EXAMPLE.p0251b.PO4Fe.SPIN 20 (fatal error on 01-02-2019: Fe scheme does not match userconfig)
./runmuffin.sh cgenie.eb_go_gs_ac_bg_sg_rg_gl.p0251b.BASESCH4 / EXAMPLE.p0251b.PO4CH4.SPIN0 20 (changed the forcing file name to "pyyyyz.RpCO2_Rp13CO2_FRALK_FDIC_F13DIC_FCa"; fatal error occurred - "cp: cannot stat `fort.2': No such file or directory")
./runmuffin.sh cgenie.eb_go_gs_ac_bg_sg_rg_gl.p0251b.BASESCH4_YC / EXAMPLE.p0251b.PO4CH4.SPIN0 20 (changed GOLDSTEINNTRACSOPTS='$(DEFINE)GOLDSTENNTRACS=18, including the defaulted temperature and salinity)


qsub -j y -o cgenie_log -V -S /bin/bash runmuffin.sh cgenie.eb_go_gs_ac_bg_sg_rg_gl.p0251b.BASESCH4 YC p0251b.PO4.DIC13CDoubleInversionCui.SPIN1 20
-------Currently running (added more ocean and sedimentary tracers)--------------------------------(Jan-03-2019)------------------
qsub -j y -o cgenie_log -V -S /bin/bash runmuffin.sh cgenie.eb_go_gs_ac_bg_sg_rg_gl.p0251b.BASESCH4_YC / EXAMPLE.p0251b.PO4CH4.SPIN0 20000
Job #21256


(1b) Second Stage SPINUP

The follow-on, 2nd-stage open system and accelerated spin-up (e.g. see Lord et al. [2015]):

./runmuffin.sh cgenie.eb_go_gs_ac_bg_sg_rg_gl.p0055c.BASES MS/gutjahretal.2017 SPIN2gl 200000 SPIN1

### model experiments -- main (no Corg burial) #################

NB. named as per Extended Data Figure 10 (Table a) and in the order that they appear in the Table.

These are run on from the end of the (2nd stage) spinup as follows:

-------------------------------------------
./runmuffin.sh cgenie.eb_go_gs_ac_bg_sg_rg_gl.p0055c.BASES MS/gutjahretal.2017 EXP.R07sm_HI 500000 SPIN2gl
./runmuffin.sh cgenie.eb_go_gs_ac_bg_sg_rg_gl.p0055c.BASES MS/gutjahretal.2017 EXP.R07sm 500000 SPIN2gl
./runmuffin.sh cgenie.eb_go_gs_ac_bg_sg_rg_gl.p0055c.BASES MS/gutjahretal.2017 EXP.R07sm_LO 500000 SPIN2gl
-------------------------------------------
./runmuffin.sh cgenie.eb_go_gs_ac_bg_sg_rg_gl.p0055c.BASES MS/gutjahretal.2017 EXP.FEsm_HI 500000 SPIN2gl
./runmuffin.sh cgenie.eb_go_gs_ac_bg_sg_rg_gl.p0055c.BASES MS/gutjahretal.2017 EXP.FEsm 500000 SPIN2gl
./runmuffin.sh cgenie.eb_go_gs_ac_bg_sg_rg_gl.p0055c.BASES MS/gutjahretal.2017 EXP.FEsm_LO 500000 SPIN2gl
-------------------------------------------
./runmuffin.sh cgenie.eb_go_gs_ac_bg_sg_rg_gl.p0055c.BASES MS/gutjahretal.2017 EXP.R07rw 500000 SPIN2gl
./runmuffin.sh cgenie.eb_go_gs_ac_bg_sg_rg_gl.p0055c.BASES MS/gutjahretal.2017 EXP.FErw 500000 SPIN2gl
-------------------------------------------
./runmuffin.sh cgenie.eb_go_gs_ac_bg_sg_rg_gl.p0055c.BASES MS/gutjahretal.2017 EXP.R07sm_noW 500000 SPIN2gl
./runmuffin.sh cgenie.eb_go_gs_ac_bg_sg_rg_gl.p0055c.BASES MS/gutjahretal.2017 EXP.FEsm_noW 500000 SPIN2gl
-------------------------------------------

### model experiments -- main (Corg burial) ####################

The Corg burial experiments listed in the Table (EXP.R07sm_Corg_HI, EXP.R07sm_Corg, EXP.R07sm_Corg_LO), 
differ from the non Corg burial experiments in that they consist of 2 segments:
(1) the onset, with no Corg burial
(2) the plateau and recovery, with Corg burial prescribed (to track the d13C recovery)
The first segment is hence the same as per EXP.R07sm_HI, EXP.R07sm, EXP.R07sm_LO.

To create the first segment:

./runmuffin.sh cgenie.eb_go_gs_ac_bg_sg_rg_gl.p0055c.BASES MS/gutjahretal.2017 EXP.R07sm_HI_Corg1 72600 SPIN2gl
./runmuffin.sh cgenie.eb_go_gs_ac_bg_sg_rg_gl.p0055c.BASES MS/gutjahretal.2017 EXP.R07sm_Corg1 72600 SPIN2gl
./runmuffin.sh cgenie.eb_go_gs_ac_bg_sg_rg_gl.p0055c.BASES MS/gutjahretal.2017 EXP.R07sm_LO_Corg1 72600 SPIN2gl

and the 2nd segment:

./runmuffin.sh cgenie.eb_go_gs_ac_bg_sg_rg_gl.p0055c.BASES MS/gutjahretal.2017 EXP.R07sm_HI_Corg2 227400 EXP.R07sm_HI_Corg1
./runmuffin.sh cgenie.eb_go_gs_ac_bg_sg_rg_gl.p0055c.BASES MS/gutjahretal.2017 EXP.R07sm_Corg2 227400 EXP.R07sm_Corg1
./runmuffin.sh cgenie.eb_go_gs_ac_bg_sg_rg_gl.p0055c.BASES MS/gutjahretal.2017 EXP.R07sm_LO_Corg2 227400 EXP.R07sm_LO_Corg1

The 2 segments of each HI/mean/LO configuration are stitched together to create a continue 300,000 year long experiment and then processed.

### model experiments -- sensitivity tests #####################

NB. as per Extended Data Figure 10 (Table b).

Experiment ID   |   duratrion of onset (yr)
-------------------------------------------
SENS.000100yr   |       100
SENS.000200yr   |       200
SENS.000500yr   |       500
SENS.001000yr   |     1,000
SENS.002000yr   |     2,000
SENS.005000yr   |     5,000
SENS.010000yr   |    10,000
SENS.020000yr   |    20,000

These are run on from the end of the (2nd stage) spinup as follows:

./runmuffin.sh cgenie.eb_go_gs_ac_bg_sg_rg_gl.p0055c.BASES MS/gutjahretal.2017 SENS.??????yr 200000 SPIN2gl

where SENS.??????yr is one of the experiment IDs listed above.

################################################################
################################################################
################################################################

install.packages("splitstackshape")
library(splitstackshape)
library(tidyverse)
library(survival)
library(survminer)

list.of.packages <- c("rstudioapi", "ggplot2", "survival", "tidyverse", "survminer" , "RColorBrewer",  "powerSurvEpi")

library(dplyr)



setwd("~/CHIC 599/Rotation 2")

NTBC_IV_July = read_csv("IVM_NTBC_July.csv")

NTBC_IV_July = NTBC_IV_July %>%
  gather(key='Treatment', value='Events', PBS_Control:NTBC_4.ng.ml)

NTBC_IV_July = expandRows(NTBC_IV_July, "Events", drop=FALSE) %>%
  group_by(Treatment) %>%
  mutate(Indx = row_number(Events))


NTBC_IV_July$Events = 1

NTBC_IV_July = NTBC_IV_July %>%
  select (-"Indx")

##Generate zeros

PBS_Control = rep(0,2)
PBS_Control = as.data.frame(PBS_Control)
PBS_Control$Treatment = "PBS_Control"
PBS_Control$Times = 432
colnames(PBS_Control)=c("Events", "Treatment", "Times")

IVM_8.5.ng.ml = rep(0,4)
IVM_8.5.ng.ml = as.data.frame(IVM_8.5.ng.ml)
IVM_8.5.ng.ml$Treatment = "IVM_8.5.ng.ml"
IVM_8.5.ng.ml$Times = 432
colnames(IVM_8.5.ng.ml)=c("Events", "Treatment", "Times")

IVM_3.5.ng.ml = rep(0,1)
IVM_3.5.ng.ml = as.data.frame(IVM_3.5.ng.ml)
IVM_3.5.ng.ml$Treatment = "IVM_3.5.ng.ml"
IVM_3.5.ng.ml$Times = 432
colnames(IVM_3.5.ng.ml)=c("Events", "Treatment", "Times")


IVM_1.5.ng.ml = rep(0,3)
IVM_1.5.ng.ml = as.data.frame(IVM_1.5.ng.ml)
IVM_1.5.ng.ml$Treatment = "IVM_1.5.ng.ml"
IVM_1.5.ng.ml$Times = 432
colnames(IVM_1.5.ng.ml)=c("Events", "Treatment", "Times")

NTBC_4.ng.ml = rep(0,3)
NTBC_4.ng.ml = as.data.frame(NTBC_4.ng.ml)
NTBC_4.ng.ml$Treatment = "NTBC_4.ng.ml"
NTBC_4.ng.ml$Times = 432
colnames(NTBC_4.ng.ml)=c("Events", "Treatment", "Times")

NTBC_IV_July=as.data.frame(NTBC_IV_July)


NTBC_IV_July_New = rbind(NTBC_IV, IVM_8.5.ng.ml, IVM_3.5.ng.ml, IVM_1.5.ng.ml, NTBC_4.ng.ml)

explain some make use in kernel makefile

https://www.gnu.org/software/make/manual/html_node/Concept-Index.html#Concept-Index_cp_letter-S

ifeq ($(KBUILD_SRC),)  ; if KBUILD_SRC is empty

ifeq ("$(origin C)", "command line")  ; param C in make commandline like: make C=0

$(filter on%, $(VAR))  ; return all match on% in VAR

$(filter-out on%, $(VAR))  ; return all NOT match on% in VAR

VAR := $(if $(filter one,$(KBUILD_SRC)),1,2) ; if $(KBUILD_SRC) has one, then VAR=1, else VAR=2

$(wildcard $(kbuild-dir)/Kbuild)  ; $(kbuild-dir)/Kbuild is file path, could have wildcard to match file

@:    ; do nothing, and don't tell or echo the command

Automatic Variables
$<    ; name of first dependent file
$@    : name of target



_all:  123
_all: all    ; make automatic combine the same target to "_all: 123 all"

$(sort foo bar lose foo)    ; order sort and remove duplicate

VAR := $(patsubst %find,%replace,myfind) ; replace find to replace in myfind, final to myreplace


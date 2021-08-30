# SPDX-License-Identifier: GPL-2.0
# Copyright(c) 1999 - 2006 Intel Corporation.

#
# Makefile for the Intel(R) PRO/1000 ethernet driver
#

obj-$(CONFIG_E1000) += e1000.o

# SPDM stuff
SPDM_DIR?= /home/marcosboger/Documents/TCC/libspdm/libspdm
SPDM_HEADERS = $(SPDM_DIR)/include
SPDM_INCLUDE =  \
        -I $(SPDM_HEADERS) \
        -I $(SPDM_HEADERS)/hal \
        -I $(SPDM_HEADERS)/hal/x64 \
        -I $(SPDM_HEADERS)/library \
        -I $(SPDM_HEADERS)/industry_standard \
        -I $(SPDM_DIR)/library/spdm_common_lib \
        -I $(SPDM_DIR)/library/spdm_requester_lib


# ccflags-y += $(SPDM_INCLUDE)
#CFLAGS_e1000.o = $(SPDM_INCLUDE)
CFLAGS_e1000_main.o = $(SPDM_INCLUDE)
CFLAGS_spdm_glue.o = $(SPDM_INCLUDE)
#CFLAGS_e1000_hw.o = $(SPDM_INCLUDE)
#CFLAGS_e1000_ethtool.o = $(SPDM_INCLUDE)
#CFLAGS_e1000_param.o = $(SPDM_INCLUDE)
#CFLAGS_spdm_glue.o = $(SPDM_INCLUDE)

SPDM_BUILD_DIR?= $(SPDM_DIR)/build

SPDM_CRYPTO?= mbedtls

SPDM_LIBS = \
        $(SPDM_BUILD_DIR)/lib/librnglib.a \
        $(SPDM_BUILD_DIR)/lib/libmemlib.a \
        $(SPDM_BUILD_DIR)/lib/libmalloclib.a \
        $(SPDM_BUILD_DIR)/lib/libdebuglib.a \
        $(SPDM_BUILD_DIR)/lib/libcryptlib_$(SPDM_CRYPTO).a \
        $(SPDM_BUILD_DIR)/lib/lib$(SPDM_CRYPTO)lib.a \
        $(SPDM_BUILD_DIR)/lib/libspdm_crypt_lib.a \
        $(SPDM_BUILD_DIR)/lib/libspdm_secured_message_lib.a \
        $(SPDM_BUILD_DIR)/lib/libspdm_requester_lib.a \
        $(SPDM_BUILD_DIR)/lib/libspdm_device_secret_lib.a \
        $(SPDM_BUILD_DIR)/lib/libspdm_common_lib.a \
        $(SPDM_BUILD_DIR)/lib/libspdm_transport_mctp_lib.a\
        $(SPDM_BUILD_DIR)/lib/libspdm_transport_pcidoe_lib.a

SPDM_DIR_SUFIX = _spdmlib
SPDM_OBJ  := $(foreach libfile, $(SPDM_LIBS), $(shell ar t $(libfile) | sed s-^-$$(basename $(libfile))$(SPDM_DIR_SUFIX)/-))
SPDM_TARGETS := $(addprefix $(src)/, $(SPDM_OBJ))

e1000-objs := e1000_main.o e1000_hw.o e1000_ethtool.o e1000_param.o spdm_glue.o $(SPDM_OBJ)

$(SPDM_TARGETS):
	mkdir -p $$(dirname $@)
	cd $$(dirname $@); ar x $(SPDM_BUILD_DIR)/lib/$$(basename $$(dirname $@ | sed s/$(SPDM_DIR_SUFIX)//)) $$(basename $@)

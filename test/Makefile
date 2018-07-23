#---------------------------------------------------------------------------------
.SUFFIXES:
#---------------------------------------------------------------------------------

ifeq ($(strip $(DEVKITPRO)),)
$(error "Please set DEVKITPRO in your environment. export DEVKITPRO=<path to>/devkitpro")
endif

TOPDIR ?= $(CURDIR)
include $(DEVKITPRO)/libnx/switch_rules

export BUILD_EXEFS_SRC := build/exefs

APP_TITLE := Chocolate-Doom
APP_DESCRIPTION := Chocolate-Doom
APP_AUTHOR := MVG
APP_VERSION := 1.0.0
ICON := Doom.jpg

CC        = aarch64-none-elf-gcc

LINKOBJ   = obj/deh_str.o obj/d_event.o obj/d_iwad.o obj/i_endoom.o obj/i_joystick.o obj/i_main.o obj/i_system.o obj/i_timer.o obj/i_video.o obj/icon.o obj/m_argv.o obj/m_bbox.o obj/m_cheat.o obj/m_config.o obj/m_controls.o obj/m_fixed.o obj/m_misc.o obj/memio.o obj/mus2mid.o obj/net_common.o obj/net_io.o obj/net_loop.o obj/net_packet.o obj/net_query.o obj/net_structrw.o obj/tables.o obj/v_video.o obj/w_checksum.o obj/w_file.o obj/w_file_stdc.o obj/w_main.o obj/w_merge.o obj/w_wad.o obj/z_zone.o   obj/d_mode.o  obj/am_map.o  obj/d_main.o obj/d_net.o obj/f_finale.o obj/g_game.o obj/info.o obj/m_random.o obj/m_menu.o obj/p_ceilng.o obj/p_doors.o obj/p_enemy.o obj/p_floor.o obj/p_inter.o obj/p_lights.o obj/p_map.o obj/p_maputl.o obj/p_mobj.o obj/p_plats.o obj/p_pspr.o obj/p_setup.o obj/p_sight.o obj/p_spec.o obj/p_switch.o obj/p_telept.o obj/p_tick.o obj/p_user.o  obj/r_data.o obj/r_draw.o obj/r_main.o obj/r_plane.o   obj/r_things.o obj/s_sound.o obj/sounds.o obj/i_sound.o obj/p_saveg.o obj/r_bsp.o obj/r_segs.o  obj/d_loop.o obj/net_client.o obj/net_server.o obj/net_sdl.o obj/deh_main.o obj/deh_io.o obj/deh_ammo.o obj/deh_frame.o obj/deh_sound.o obj/deh_thing.o obj/deh_weapon.o obj/deh_mapping.o obj/sha1.o obj/i_sdlsound.o obj/i_input.o obj/i_sdlmusic.o obj/v_diskicon.o obj/doomstat.o obj/d_items.o obj/hu_stuff.o	obj/hu_lib.o obj/r_sky.o obj/deh_doom.o	obj/deh_misc.o obj/st_stuff.o obj/st_lib.o obj/f_wipe.o obj/deh_cheat.o obj/deh_ptr.o obj/deh_text.o obj/dstrings.o obj/wi_stuff.o obj/statdump.o obj/deh_bexstr.o

#OPL Music
LINKOBJ   += obj/opl.o obj/opl_sdl.o obj/i_oplmusic.o obj/opl_queue.o obj/opl3.o obj/midifile.o

#---------------------------------------------------------------------------------
# TARGET is the name of the output
# BUILD is the directory where object files & intermediate files will be placed
# SOURCES is a list of directories containing source code
# DATA is a list of directories containing data files
# INCLUDES is a list of directories containing header files
# EXEFS_SRC is the optional input directory containing data copied into exefs, if anything this normally should only contain "main.npdm".
# ROMFS is the directory containing data to be added to RomFS, relative to the Makefile (Optional)
#
# NO_ICON: if set to anything, do not use icon.
# NO_NACP: if set to anything, no .nacp file is generated.
# APP_TITLE is the name of the app stored in the .nacp file (Optional)
# APP_AUTHOR is the author of the app stored in the .nacp file (Optional)
# APP_VERSION is the version of the app stored in the .nacp file (Optional)
# APP_TITLEID is the titleID of the app stored in the .nacp file (Optional)
# ICON is the filename of the icon (.jpg), relative to the project folder.
#   If not set, it attempts to use one of the following (in this order):
#     - <Project name>.jpg
#     - icon.jpg
#     - <libnx folder>/default_icon.jpg
#---------------------------------------------------------------------------------
TARGET		:=	$(notdir $(CURDIR))
BUILD		:=	build
SOURCES		:=	src
DATA		:=	data
INCLUDES	:=	include
EXEFS_SRC	:=	exefs_src
#ROMFS	:=	romfs

#---------------------------------------------------------------------------------
# options for code generation
#---------------------------------------------------------------------------------
ARCH	:=	-march=armv8-a -mtune=cortex-a57 -mtp=soft -fPIE

CFLAGS	:=	-g -Wall -O2 -ffunction-sections \
			$(ARCH) $(DEFINES)

CFLAGS	+=	$(INCLUDE) -D__SWITCH__

CXXFLAGS	:= $(CFLAGS) -fno-rtti -fno-exceptions -std=gnu++11

ASFLAGS	:=	-g $(ARCH)
LDFLAGS	=	-specs=$(DEVKITPRO)/libnx/switch.specs -g $(ARCH) -Wl,-Map,$(notdir $*.map)

LIBS	:= -lnx

LIBS    +=  -mtune=cortex-a57 -mtp=soft -fPIE -L$(DEVKITPRO)/libnx/lib -L$(DEVKITPRO)/portlibs/switch/lib  -lSDL2_image -lpng -ljpeg -logg -lmodplug -lmpg123 -lSDL2  -lm  -lstdc++ -lnx -lz
INCS    +=  -Isrc -Icodeblocks -Iopl -Itextscreen -I$(DEVKITPRO)/portlibs/switch/include/SDL2 -I"src/main" -I$(DEVKITPRO)/libnx/include -I$(DEVKITPRO)/portlibs/switch/include  
CXXINCS := -I$(DEVKITPRO)/portlibs/switch/include/SDL2 -I"src/main" -I$(DEVKITPRO)/libnx/include -I$(DEVKITPRO)/portlibs/switch/include

#---------------------------------------------------------------------------------
# list of directories containing libraries, this must be the top level containing
# include and lib
#---------------------------------------------------------------------------------
LIBDIRS	:= $(PORTLIBS) $(LIBNX)


#---------------------------------------------------------------------------------
# no real need to edit anything past this point unless you need to add additional
# rules for different file extensions
#---------------------------------------------------------------------------------
ifneq ($(BUILD),$(notdir $(CURDIR)))
#---------------------------------------------------------------------------------

export OUTPUT	:=	$(CURDIR)/$(TARGET)
export TOPDIR	:=	$(CURDIR)

export VPATH	:=	$(foreach dir,$(SOURCES),$(CURDIR)/$(dir)) \
			$(foreach dir,$(DATA),$(CURDIR)/$(dir))

export DEPSDIR	:=	$(CURDIR)/$(BUILD)

CFILES		:=	$(foreach dir,$(SOURCES),$(notdir $(wildcard $(dir)/*.c)))
CPPFILES	:=	$(foreach dir,$(SOURCES),$(notdir $(wildcard $(dir)/*.cpp)))
SFILES		:=	$(foreach dir,$(SOURCES),$(notdir $(wildcard $(dir)/*.s)))
BINFILES	:=	$(foreach dir,$(DATA),$(notdir $(wildcard $(dir)/*.*)))

#---------------------------------------------------------------------------------
# use CXX for linking C++ projects, CC for standard C
#---------------------------------------------------------------------------------
ifeq ($(strip $(CPPFILES)),)
#---------------------------------------------------------------------------------
	export LD	:=	$(CC)
#---------------------------------------------------------------------------------
else
#---------------------------------------------------------------------------------
	export LD	:=	$(CXX)
#---------------------------------------------------------------------------------
endif
#---------------------------------------------------------------------------------

export OFILES_BIN	:=	$(addsuffix .o,$(BINFILES))
export OFILES_SRC	:=	$(CPPFILES:.cpp=.o) $(CFILES:.c=.o) $(SFILES:.s=.o)
export OFILES 	:=	$(OFILES_BIN) $(OFILES_SRC)
export HFILES_BIN	:=	$(addsuffix .h,$(subst .,_,$(BINFILES)))

export INCLUDE	:=	$(foreach dir,$(INCLUDES),-I$(CURDIR)/$(dir)) \
			$(foreach dir,$(LIBDIRS),-I$(dir)/include) \
			-I$(CURDIR)/$(BUILD)

export LIBPATHS	:=	$(foreach dir,$(LIBDIRS),-L$(dir)/lib)

export BUILD_EXEFS_SRC := $(TOPDIR)/$(EXEFS_SRC)

ifeq ($(strip $(ICON)),)
	icons := $(wildcard *.jpg)
	ifneq (,$(findstring $(TARGET).jpg,$(icons)))
		export APP_ICON := $(TOPDIR)/$(TARGET).jpg
	else
		ifneq (,$(findstring icon.jpg,$(icons)))
			export APP_ICON := $(TOPDIR)/icon.jpg
		endif
	endif
else
	export APP_ICON := $(TOPDIR)/$(ICON)
endif

ifeq ($(strip $(NO_ICON)),)
	export NROFLAGS += --icon=$(APP_ICON)
endif

ifeq ($(strip $(NO_NACP)),)
	export NROFLAGS += --nacp=$(CURDIR)/$(TARGET).nacp
endif

ifneq ($(APP_TITLEID),)
	export NACPFLAGS += --titleid=$(APP_TITLEID)
endif

ifneq ($(ROMFS),)
	export NROFLAGS += --romfsdir=$(CURDIR)/$(ROMFS)
endif

.PHONY: $(BUILD) clean all

#---------------------------------------------------------------------------------
all: $(BUILD)

$(BUILD):
	@[ -d $@ ] || mkdir -p $@
	@$(MAKE) --no-print-directory -C $(BUILD) -f $(CURDIR)/Makefile

#---------------------------------------------------------------------------------
clean:
	@echo clean ...
	@rm -fr $(BUILD) $(TARGET).pfs0 $(TARGET).nso $(TARGET).nro $(TARGET).nacp $(TARGET).elf


#---------------------------------------------------------------------------------
else
.PHONY:	all

DEPENDS	:=	$(OFILES:.o=.d)

#---------------------------------------------------------------------------------
# main targets
#---------------------------------------------------------------------------------
all	:	$(OUTPUT).pfs0 $(OUTPUT).nro

$(OUTPUT).pfs0	:	$(OUTPUT).nso

$(OUTPUT).nso	:	$(OUTPUT).elf

ifeq ($(strip $(NO_NACP)),)
$(OUTPUT).nro	:	$(OUTPUT).elf $(OUTPUT).nacp
else
$(OUTPUT).nro	:	$(OUTPUT).elf
endif

$(OUTPUT).elf	:	$(OFILES)

$(OFILES_SRC)	: $(HFILES_BIN)

#---------------------------------------------------------------------------------
# you need a rule like this for each extension you use as binary data
#---------------------------------------------------------------------------------
%.bin.o	%_bin.h :	%.bin
#---------------------------------------------------------------------------------
	@echo $(notdir $<)
	@$(bin2o)

-include $(DEPENDS)

#---------------------------------------------------------------------------------------
endif
#---------------------------------------------------------------------------------------
cp sdlpop.nro "C:\Users\Albert\Downloads\ryujinx-1.0.1163-win_x64\publish"

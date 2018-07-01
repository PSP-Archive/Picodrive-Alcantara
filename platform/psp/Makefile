
# pspdev is expected to be in path
PSPSDK = $(shell psp-config --pspsdk-path)

# settings
#use_musashi = 1
#use_mz80 = 1
amalgamate = 0
for_15fw = 0
release = 1
profile = 0
dynarecs = 1
# :!touch platform/psp/psp.c

CFLAGS += -IC:/pspsdk/powermgr/src/include
CFLAGS += -IC:/pspsdk/psp/sdk/include/libc
#CFLAGS += -I../.. -I. -DNO_SYNC -D_ASM_DRAW_C_AMIPS
CFLAGS += -I../.. -I. -DNO_SYNC 
CFLAGS += -Wall -Winline -G0

#SVP dynarec
ifeq "$(dynarecs)" "1"
#CFLAGS += -D_SVP_DRC -DDRC_SH2
CFLAGS += -D_SVP_DRC
endif

#CFLAGS += -DLPRINTF_STDIO

ifeq "$(profile)" "1"
CFLAGS += -pg -DGPROF
endif

CFLAGS += -LC:/pspsdk/powermgr/src/lib

ifeq "$(release)" "1"
#release
#ifeq ($(DEBUG),)
CFLAGS += -O2 -ftracer -fstrength-reduce -ffast-math
else
CFLAGS += -g3 -O0
endif
ifeq "$(for_15fw)" "1"
CFLAGS += -DFW15
endif


# frontend and stuff
OBJS += emu_psp.o mp3_psp.o psp.o asm_utils.o plat_psp.o

# libpicofe
OBJS += ../../platform/libpicofe_/fonts.o  \
		../../platform/libpicofe_/readpng.o ../../platform/libpicofe_/input.o \
		../../platform/libpicofe_/psp/in_psp.o 		  

# common
OBJS += ../../platform/common/main.o ../../platform/common/emu.o ../../platform/common/menu_pico.o \
		../../platform/common/config_file.o 

# Pico
ifeq "$(amalgamate)" "1"
OBJS += ../../picoAll.o
else
OBJS += ../../pico/cart.o ../../pico/memory.o ../../pico/misc.o ../../pico/pico.o ../../pico/sek.o ../../pico/videoport.o \
	    ../../pico/draw2.o ../../pico/draw.o ../../pico/z80if.o ../../pico/patch.o \
	    ../../pico/misc_amips.o ../../pico/debug.o ../../pico/state.o ../../pico/media.o ../../pico/sms.o ../../pico/eeprom.o \
	    ../../pico/mode4.o ../../pico/memory_amips.o ../../pico/draw2_amips.o
#	    ../../pico/mode4.o   ../../pico/draw_amips.o
# Pico - CD
OBJS += ../../pico/cd/cd_memory.o ../../pico/cd/cd_sek.o \
		../../pico/cd/cue.o ../../pico/cd/gfx.o \
		../../pico/cd/cd_misc.o ../../pico/cd/pcm.o ../../pico/cd/mcd.o \
		../../pico/cd/gfx_dma.o ../../pico/cd/cdd.o \
		../../pico/cd/cdc.o ../../pico/cd/cd_image.o ../../pico/cd/cell_map.o		
#		../../pico/cd/cdc.o ../../pico/cd/cd_image.o ../../pico/cd/cell_map.o  ../../pico/cd/memory_amips.o		
		
# Pico - 32x
OBJS += ../../pico/32x/32x.o ../../pico/32x/32x_draw.o \
		../../pico/32x/32x_memory.o ../../pico/32x/pwm.o \
		../../pico/32x/sh2soc.o		
# Pico - carthw
OBJS += ../../pico/carthw/carthw.o ../../pico/carthw/eeprom_spi.o \
		../../pico/carthw/svp/ssp16.o ../../pico/carthw/svp/svp.o ../../pico/carthw/svp/svp_memory.o
		
ifeq "$(dynarecs)" "1"		
OBJS += ../../pico/carthw/svp/compiler_mips.o ../../pico/carthw/svp/stub_amips.o ../../cpu/drc/cmn.o cache.o 
		
endif		
		
# Pico - Pico
OBJS += ../../pico/pico/pico_pico.o ../../pico/pico/pico_memory.o ../../pico/pico/xpcm.o
endif

# Pico - sound
ifneq "$(amalgamate)" "1"
OBJS += ../../pico/sound/sound.o
endif
OBJS += ../../pico/sound/mix.o
#OBJS += ../../pico/sound/mix_amips.o
#OBJS += ../../pico/sound/sn76496.o ../../pico/sound/ym2612.o ../../pico/sound/ym2612_amips.o
OBJS += ../../pico/sound/sn76496.o ../../pico/sound/ym2612.o
# zlib (hacked)
OBJS += ../../zlib/gzio.o ../../zlib/inffast.o ../../zlib/inflate.o ../../zlib/inftrees.o ../../zlib/trees.o \
	../../zlib/deflate.o ../../zlib/crc32.o ../../zlib/adler32.o ../../zlib/zutil.o ../../zlib/compress.o \
	../../zlib/uncompr.o
# unzip
OBJS += ../../unzip/unzip.o ../../unzip/unzip_stream.o
# CPU cores
ifeq "$(use_musashi)" "1"
CFLAGS += -DEMU_M68K
OBJS += ../../cpu/musashi/m68kops.o ../../cpu/musashi/m68kcpu.o 
else
CFLAGS += -DEMU_F68K
OBJS += ../../cpu/fame/famec.o
endif
# sh2    
OBJS += ../../cpu/sh2/sh2.o ../../cpu/sh2/compiler_sh2_mips.o \
        ../../cpu/sh2/mame/sh2pico.o        
# z80
ifeq "$(use_mz80)" "1"
CFLAGS += -D_USE_MZ80
OBJS += ../../cpu/mz80/mz80.o
else
CFLAGS += -D_USE_CZ80
OBJS += ../../cpu/cz80/cz80.o
endif
# bg images
OBJS += data/bg32.o data/bg40.o

vpath %.c = ../..
vpath %.s = ../..
DIRS = platform platform/psp platform/common pico pico/cd pico/pico pico/sound pico/carthw/svp \
	zlib unzip cpu cpu/musashi cpu/fame cpu/mz80 cpu/cz80


LIBS += -lpng -lm -lpspgu -lpsppower -lpspaudio -lpsprtc -lpspaudiocodec -lpspkubridge 
ifeq "$(profile)" "1"
LIBS += -lpspprof
endif
ifeq "$(release)" "0"
LIBS += -lpspgdb_kernel -lpspgdb_user
endif
LDFLAGS += -Wl,-Map=PicoDrive.map


# target
TARGET = PicoDrive
#EXTRA_TARGETS = mkdirs EBOOT.PBP
EXTRA_TARGETS = EBOOT.PBP
PSP_EBOOT_TITLE = PicoDrive 1.92
PSP_EBOOT_ICON = data/icon.png
#PSP_EBOOT_PIC1 = .png
ifeq "$(for_15fw)" "0"
ifeq "$(profile)" "0"
BUILD_PRX = 1
endif
endif

CUSTOM_CLEAN = myclean

include $(PSPSDK)/lib/build.mak

# some additional rules
mkdirs:
	mkdir -p $(DIRS)

.c.o:
	@echo ">>>" $<
	$(CC) $(CFLAGS) -c $< -o $@

AS := psp-as

.s.o:
	@echo ">>>" $<
	$(AS) -march=allegrex -mtune=allegrex $< -o $@

../../cpu/musashi/m68kops.c :
	make -C ../../cpu/musashi

../../cpu/fame/famec.o : ../../cpu/fame/famec.c
	@echo ">>>" $<
	$(CC) $(CFLAGS) -Wno-unused -c $< -o $@
	
../../cpu/sh2/sh2.o : ../../cpu/sh2/sh2.c	
	@echo ">>>" $<
	$(CC) $(CFLAGS) -O3 -c $< -o $@

ifeq "$(release)" "1"
../../pico/carthw/svp/compiler_mips.o : ../../pico/carthw/svp/compiler_mips.c
	@echo ">>>" $<
	$(CC) $(CFLAGS) -O3 -c $< -o $@

../../pico/sound/sound.o : ../../pico/sound/sound.c
	@echo ">>>" $<
	$(CC) $(CFLAGS) -O3 -c $< -o $@ 

../../pico/sound/ym2612.o : ../../pico/sound/ym2612.c
	@echo ">>>" $<
	$(CC) $(CFLAGS) -O3 -c $< -o $@
endif

../../pico/misc.o : ../../pico/misc.c
	@echo ">>>" $<
	$(CC) $(CFLAGS) -c $< -o $@ -D_ASM_MISC_C_AMIPS
	
../../pico/draw2.o : ../../pico/draw2.c
	@echo ">>>" $<
	$(CC) $(CFLAGS) -O2 -c $< -o $@ -D_ASM_DRAW_C	

#../../pico/memory.o : ../../pico/memory.c
#	@echo ">>>" $<
#	$(CC) $(CFLAGS) -O2 -c $< -o $@

../../pico/memory.o : ../../pico/memory.c
	@echo ">>>" $<
	$(CC) $(CFLAGS) -O2 -c $< -o $@ -D_ASM_MEMORY_C -D_ASM_MEMORY_C_AMIPS

../../pico/cd/memory.o : ../../pico/cd/cd_memory.c
	@echo ">>>" $<
	$(CC) $(CFLAGS) -O2 -c $< -o $@

../../pico/cd/gfx_cd.o : ../../pico/cd/gfx.c
	@echo ">>>" $<
	$(CC) $(CFLAGS) -O2 -c $< -o $@

readme.txt: ../../tools/textfilter ../base_readme.txt
	../../tools/textfilter ../base_readme.txt $@ PSP

../../tools/textfilter: ../../tools/textfilter.c
	make -C ../../tools/ textfilter

data/bg32.o: data/bg32.bin
	bin2o -i $< $@ bgdatac32

data/bg40.o: data/bg40.bin
	bin2o -i $< $@ bgdatac40

#
ifndef UPDIR
UPDIR = /media/disk/PSP/GAME/PicoDrive/
endif
up: EBOOT.PBP
	@cp -v $^ $(UPDIR)


# cleanup

myclean:
	#rm -rf $(DIRS)
	$(RM) PicoDrive.map
	make -C ../../cpu/musashi clean


clean_prof:
	find ../.. -name '*.gcno' -delete
	find ../.. -name '*.gcda' -delete

# ----------- release -----------
ifneq ($(findstring rel,$(MAKECMDGOALS)),)
ifeq ($(VER),)
$(error need VER)
endif
endif

# ?
rel: EBOOT.PBP readme.txt ../game_def.cfg
	mkdir -p PicoDrive/skin/
	cp $^ PicoDrive/
	cp skin/* PicoDrive/skin/
	zip -9 -r ../../PicoDrive_psp_$(VER).zip PicoDrive
	rm -rf PicoDrive
	mkdir bin_to_cso_mp3
	cp ../../tools/bin_to_cso_mp3/* bin_to_cso_mp3/
	zip -9 -r ../../PicoDrive_psp_$(VER).zip bin_to_cso_mp3
	rm -rf bin_to_cso_mp3

rel_kxploit: readme.txt ../game_def.cfg
	mkdir -p PicoDrive/skin/
	cp $^ PicoDrive/
	cp skin/* PicoDrive/skin/
	zip -9 -r ../../PicoDrive_psp_$(VER)_kxploit.zip PicoDrive
	zip -9 -r ../../PicoDrive_psp_$(VER)_kxploit.zip PicoDrive%
	mkdir bin_to_cso_mp3
	cp ../../tools/bin_to_cso_mp3/* bin_to_cso_mp3/
	zip -9 -r ../../PicoDrive_psp_$(VER)_kxploit.zip bin_to_cso_mp3
	rm -rf bin_to_cso_mp3


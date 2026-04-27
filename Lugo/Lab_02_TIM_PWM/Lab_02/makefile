## makefile
## ---------------------------------------------------------------------------
## Build script – STM32 Nucleo-F411RE, bare-metal C with CMSIS.
##
##   make build   → compiles and links   → build/app.elf
##   make flash   → program via OpenOCD
##   make clean   → deletes objects and ELF
## ---------------------------------------------------------------------------

include sources.mk
OPENOCD = "C:/ST/STM32CubeIDE_2.0.0/STM32CubeIDE/plugins/com.st.stm32cube.ide.mcu.externaltools.openocd.win32_2.4.300.202509300731/tools/bin/openocd.exe"

OPENOCD_SCRIPTS = "C:/ST/STM32CubeIDE_2.0.0/STM32CubeIDE/plugins/com.st.stm32cube.ide.mcu.debug.openocd_2.3.200.202510310951/resources/openocd/st_scripts"
## ── Output ──────────────────────────────────────────────────────────────────
EXEC = build/app.elf
OBJS = $(SRC:.c=.o)

## ── Linker script ───────────────────────────────────────────────────────────
LINKER_FILE = linker/stm32411re.ld

## ── Target ──────────────────────────────────────────────────────────────────
CPU   = cortex-m4
ARCH  = armv7e-m
FPU   = fpv4-sp-d16
SPECS = nosys.specs

ARCH_FLAGS = -mcpu=$(CPU) -mthumb -march=$(ARCH) -mfloat-abi=hard -mfpu=$(FPU) --specs=$(SPECS)

## ── Flags ───────────────────────────────────────────────────────────────────
CFLAGS  = -g -O0 -std=c99 -Werror -Wall -DSTM32F411xE $(ARCH_FLAGS)
LDFLAGS = -nostdlib -T $(LINKER_FILE)

CC = arm-none-eabi-gcc

## ── Rules ──────────────────────────────────────────────────────────────────
%.o : %.c
	$(CC) -c $< $(CFLAGS) $(INCLUDES) -o $@

.PHONY: build
build: $(EXEC)
	rm -f $(OBJS)

$(EXEC): $(OBJS)
	@mkdir -p build
	$(CC) $(OBJS) $(CFLAGS) $(INCLUDES) $(LDFLAGS) -o $@

.PHONY: flash
flash:
	$(OPENOCD) -s $(OPENOCD_SCRIPTS) \
	-f interface/stlink-dap.cfg \
	-f target/stm32f4x.cfg \
	-c "program $(EXEC) verify reset" \
	-c shutdown

.PHONY: clean
clean:
	rm -f $(OBJS) $(EXEC)

CMAKE_MINIMUM_REQUIRED(VERSION 3.10)

SET(CMAKE_SYSTEM_NAME Generic)
SET(CMAKE_SYSTEM_PROCESSOR cortex-m4)
#SET(CMAKE_VERBOSE_MAKEFILE ON)

SET(CMAKE_EXPORT_COMPILE_COMMANDS ON)

#SET(CMAKE_C_COMPILER "D:/RT-ThreadStudio/platform/env_released/env/tools/ConEmu/ConEmu/../../../tools/gnu_gcc/arm_gcc/mingw/bin/arm-none-eabi-gcc.exe")
#SET(CMAKE_ASM_COMPILER "D:/RT-ThreadStudio/platform/env_released/env/tools/ConEmu/ConEmu/../../../tools/gnu_gcc/arm_gcc/mingw/bin/arm-none-eabi-gcc.exe")
#SET(CMAKE_C_FLAGS "")
#SET(CMAKE_ASM_FLAGS "")
SET(CMAKE_C_COMPILER_WORKS TRUE)

#SET(CMAKE_CXX_COMPILER "D:/RT-ThreadStudio/platform/env_released/env/tools/ConEmu/ConEmu/../../../tools/gnu_gcc/arm_gcc/mingw/bin/arm-none-eabi-g++.exe")
#SET(CMAKE_CXX_FLAGS "")
SET(CMAKE_CXX_COMPILER_WORKS TRUE)

#SET(CMAKE_OBJCOPY "D:/RT-ThreadStudio/platform/env_released/env/tools/ConEmu/ConEmu/../../../tools/gnu_gcc/arm_gcc/mingw/bin/arm-none-eabi-objcopy.exe")
#SET(CMAKE_SIZE "D:/RT-ThreadStudio/platform/env_released/env/tools/ConEmu/ConEmu/../../../tools/gnu_gcc/arm_gcc/mingw/bin/arm-none-eabi-size.exe")

#SET(CMAKE_EXE_LINKER_FLAGS "-T ${CMAKE_SOURCE_DIR}/linkscripts/AT32F435ZM/link.lds")


set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -mcpu=cortex-m4 -mthumb -O2 -g")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -mcpu=cortex-m4 -mthumb -O2 -g")

SET(CMAKE_EXE_LINKER_FLAGS "-T ${CMAKE_SOURCE_DIR}/linkscripts/AT32F435ZM/link.lds")

SET(CMAKE_CXX_STANDARD 14)
PROJECT(rtthread C CXX ASM)
INCLUDE_DIRECTORIES(
		.
		drivers
		drivers/f435_437/include
		drivers/common/include
		drivers/usbotg_library/inc
		libraries/f435_437/cmsis/cm4/core_support
		libraries/f435_437/cmsis/cm4/device_support
		libraries/f435_437/drivers/inc
	rt-thread/components/libc/compilers/common/include
	rt-thread/components/libc/compilers/newlib
	rt-thread/libcpu/arm/common
	rt-thread/libcpu/arm/cortex-m4
	rt-thread/components/drivers/include
	rt-thread/components/finsh
	rt-thread/include
	rt-thread/components/libc/posix/io/poll
	rt-thread/components/libc/posix/io/stdio
	rt-thread/components/libc/posix/ipc
)

ADD_DEFINITIONS(
	-DHAVE_CCONFIG_H
	-DRT_USING_LIBC
	-DRT_USING_NEWLIB
	-D_POSIX_C_SOURCE=1
	-D__RTTHREAD__
	-DAT32F435ZMT7
    -DSOC_SERIES_AT32F435
)

SET(PROJECT_SOURCES
	rt-thread/components/libc/compilers/common/cctype.c
	rt-thread/components/libc/compilers/common/cstdio.c
	rt-thread/components/libc/compilers/common/cstdlib.c
	rt-thread/components/libc/compilers/common/cstring.c
	rt-thread/components/libc/compilers/common/ctime.c
	rt-thread/components/libc/compilers/common/cwchar.c
	rt-thread/components/libc/compilers/newlib/syscalls.c
	rt-thread/libcpu/arm/common/backtrace.c
	rt-thread/libcpu/arm/common/div0.c
	rt-thread/libcpu/arm/common/showmem.c
	rt-thread/libcpu/arm/cortex-m4/context_gcc.S
	rt-thread/libcpu/arm/cortex-m4/cpuport.c
	rt-thread/components/drivers/ipc/completion.c
	rt-thread/components/drivers/ipc/dataqueue.c
	rt-thread/components/drivers/ipc/pipe.c
	rt-thread/components/drivers/ipc/ringblk_buf.c
	rt-thread/components/drivers/ipc/ringbuffer.c
	rt-thread/components/drivers/ipc/waitqueue.c
	rt-thread/components/drivers/ipc/workqueue.c
	rt-thread/components/drivers/misc/pin.c
	rt-thread/components/drivers/serial/serial.c
	rt-thread/components/finsh/shell.c
	rt-thread/components/finsh/msh.c
	rt-thread/components/finsh/msh_parse.c
	rt-thread/components/finsh/cmd.c
	rt-thread/src/clock.c
	rt-thread/src/components.c
	rt-thread/src/device.c
	rt-thread/src/idle.c
	rt-thread/src/ipc.c
	rt-thread/src/irq.c
	rt-thread/src/kservice.c
	rt-thread/src/mem.c
	rt-thread/src/mempool.c
	rt-thread/src/object.c
	rt-thread/src/scheduler.c
	rt-thread/src/thread.c
	rt-thread/src/timer.c
	applications/main.c
		drivers/board.c
		drivers/f435_437/drv_clk.c
		drivers/f435_437/at32_msp.c
		libraries/f435_437/cmsis/cm4/device_support/startup/gcc/startup_at32f435_437.s
		libraries/f435_437/drivers/src/at32f435_437_crm.c
		libraries/f435_437/drivers/src/at32f435_437_gpio.c
		libraries/f435_437/drivers/src/at32f435_437_misc.c
		libraries/f435_437/drivers/src/at32f435_437_exint.c
		libraries/f435_437/drivers/src/at32f435_437_scfg.c
		libraries/f435_437/drivers/src/at32f435_437_usart.c
		libraries/f435_437/cmsis/cm4/device_support/system_at32f435_437.c
		drivers/common/drv_common.c
		drivers/common/drv_gpio.c
		drivers/common/drv_usart.c
		)

LINK_DIRECTORIES(
)

LINK_LIBRARIES(
	c
	m
)

ADD_EXECUTABLE(${CMAKE_PROJECT_NAME}.elf ${PROJECT_SOURCES})
ADD_CUSTOM_COMMAND(TARGET ${CMAKE_PROJECT_NAME}.elf POST_BUILD 
COMMAND ${CMAKE_OBJCOPY} -O binary ${CMAKE_PROJECT_NAME}.elf ${CMAKE_PROJECT_NAME}.bin COMMAND ${CMAKE_SIZE} ${CMAKE_PROJECT_NAME}.elf)
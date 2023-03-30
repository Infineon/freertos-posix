# FreeRTOS POSIX library for Infineon MCUs

## Overview
The Portable Operating System Interface (POSIX) is a family of standards specified by the IEEE Computer Society for maintaining compatibility between operating systems. freertos-posix implements *a small subset* of the [POSIX threading](https://pubs.opengroup.org/onlinepubs/7908799/xsh/threads.html) API. This subset allows application developers familiar with POSIX API to develop a FreeRTOS application using POSIX-like threading primitives. freertos-posix implement around only 80% of the POSIX API. Therefore, an existing POSIX-compliant application or a POSIX-compliant library cannot be ported to run on FreeRTOS Kernel using only these wrappers.

## Quick start

The quick start guide provides steps to create a simple blinking LED project with a single task using pthread.

This quick start guide assumes that the environment is configured to use the board support package (BSP) for your kit and it is included in the project.

Do the following to create a simple POSIX application:

1. Add freertos-posix to the project. For ModusToolbox&trade; software, enable freertos-posix using the library manager.
2. Add `FREERTOS` to the `COMPONENTS` variable defined in the ModusToolbox&trade; application Makefile:
    ```c
    COMPONENTS+=FREERTOS
    ```
3. Add the following options to `FreeRTOSConfig.h`:
    ```
    #define configUSE_POSIX_ERRNO                   1
    #define configUSE_APPLICATION_TASK_TAG          1
    #define INCLUDE_xSemaphoreGetMutexHolder        1
    ```
4. Do the following in the `main.c` file:
    1. Include the required header files:
    ```c
        #include "FreeRTOS_POSIX.h"
        #include "FreeRTOS_POSIX/unistd.h"
        #include "FreeRTOS_POSIX/pthread.h"
        #include "cyhal.h"
        #include "cybsp.h"
    ```
    `Note`: POSIX header files should be included first in the file.

   2. Specify the GPIO port and pin for the LED:

        ```c
        #define LED_GPIO (CYBSP_USER_LED)
        ```

    3. Add the function to toggle the LED:

        ```c
        void* blinky_thread(void * arg)
        {
            /* Initialize the LED GPIO pin */
            cyhal_gpio_init(LED_GPIO, CYHAL_GPIO_DIR_OUTPUT,
                            CYHAL_GPIO_DRIVE_STRONG, CYBSP_LED_STATE_OFF);

            for(;;)
            {
                /* Toggle the LED every 1 second */
                cyhal_gpio_toggle(LED_GPIO);
                sleep(1);
            }
            return arg;
        }
        ```

    4. Create a task using the previously added LED toggle function and start the task scheduler:

        ```c

        #define BLINKY_STACK_SZ (1 *1024)

        int main(void)
        {
            int retval = 0;
            cy_rslt_t result;
            pthread_t pxID;
            pthread_attr_t pxAttr;

            /* Initialize the device and board peripherals */
            result = cybsp_init() ;
            if (result != CY_RSLT_SUCCESS)
            {
                CY_ASSERT(0);
            }

            __enable_irq();

            /* Initialize Thread Attributes */
            pthread_attr_init(&pxAttr);
            pthread_attr_setstacksize(&pxAttr, BLINKY_STACK_SZ);

            retval = pthread_create( &pxID , &pxAttr, blinky_thread, NULL );
            if (0 == retval)
            {
                vTaskStartScheduler();
            }

            for (;;)
            {
                /* vTaskStartScheduler never returns */
            }
        }
        ```

6. Build the project and program it into the target kit.

7. Observe the LED blinking on the kit.

## Configuration considerations
To configure FreeRTOS, copy the pre-configured *FreeRTOSConfig.h* file from the *freertos/Source/portable* folder to your project and modify the copied configuration file as needed.

## Notes

1. This library forked from https://github.com/FreeRTOS/Lab-Project-FreeRTOS-POSIX.

2. This is beta-quality code.

## More information

- [RELEASE.md](./RELEASE.md)
- [POSIX API Reference](http://pubs.opengroup.org/onlinepubs/9699919799/)
- [ModusToolbox&trade; software environment, quick start guide, documentation, and videos](https://www.infineon.com/cms/en/design-support/tools/sdk/modustoolbox-software)
- [Infineon website](https://www.infineon.com/)
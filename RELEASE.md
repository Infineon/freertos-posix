# FreeRTOS POSIX library for Infineon MCUs

FreeRTOS POSIX library provides the POSIX implementation on top of freertos.

## What's Included?

See [README.md](./README.md) for a complete description of freertos-posix features.

## Known Issues
| Problem | Workaround |
| ------- | ---------- |
| Passing invalid thread id argument to pthread_join results in undefined behavior | Currently, no workaround is available. User shall use the thread id obtained from pthread_create |

## Changelog

### v1.0.0
* Initial release.

## Supported Software and Tools

The current version of the library was validated for compatibility with the following Software and Tools:

| Software and tools                                      | Version |
| :---                                                    | :----:  |
| ModusToolbox&trade; software environment                | 3.0     |
| FreeRTOS                                                | 10.4.3  |
| Peripheral driver library (`mtb-pdl-cat1`)              | 3.2.0   |
| GCC compiler                                            | 10.3.1  |
| IAR compiler                                            | 9.30    |
| Arm&reg; compiler 6                                     | 6.16    |

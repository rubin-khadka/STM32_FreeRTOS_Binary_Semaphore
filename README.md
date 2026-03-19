# STM32 FreeRTOS Binary Semaphore

## Overview
This project demonstrates the usage of binary semaphores for task synchronization in an STM32 microcontroller using FreeRTOS (CMSIS-RTOS v1). Binary semaphores are used to manage access to shared resources and synchronize task execution in real-time embedded systems.

## Project Description
The application creates three tasks with different priorities:
- HighTask (Priority: Above Normal)
- NormalTask (Priority: Normal)
- LowTask (Priority: Below Normal)

A binary semaphore is used to control access to a critical section, demonstrating mutual exclusion and task blocking/synchronization behavior.

## Task Behavior
### NormalTask (Medium Priority)
1. Prints "Entered NormalTask and waiting for semaphore"
2. Waits indefinitely for binary semaphore
3. Once acquired, prints "Semaphore acquired by Normal Task"
4. Enters a waiting loop until button on PA1 is pressed
5. When button pressed, prints "Leaving NormalTask and releasing Semaphore"
6. Releases the semaphore
7. Delays 500ms before repeating

### HighTask (Highest Priority)
1. Prints "Entered HighTask and waiting for Semaphore"
2. Waits indefinitely for binary semaphore
3. Upon acquiring semaphore, prints "Semaphore acquired by High Task"
4. Immediately prints "Leaving HighTask and releasing Semaphore"
5. Releases the semaphore
6. Delays 500ms before repeating

### LowTask (Lowest Priority)
1. Prints "Entered LowTask"
2. Immediately prints "Leaving LowTask"
3. Delays 500ms before repeating
4. Does NOT use the semaphore (demonstrates independent execution)

## Binary Semaphore Behavior Demonstrated
1. Mutual Exclusion
    - Only one task can hold the semaphore at any given time
    - When NormalTask holds the semaphore, HighTask is blocked even though it has higher priority
    - This demonstrates how semaphores protect shared resources from concurrent access

2. Task Blocking
    - Tasks attempting to take a semaphore that is unavailable are placed in the Blocked state
    - Blocked tasks consume no CPU time
    - Tasks are automatically unblocked when the semaphore becomes available

3. Priority Inheritance
    - When HighTask (higher priority) waits for a semaphore held by NormalTask (lower priority), the RTOS may temporarily boost NormalTask's priority to prevent priority inversion
    - This ensures the higher priority task doesn't wait indefinitely

4. Synchronization Sequence
```
1. System starts → Highest priority task (HighTask) runs first
2. HighTask attempts to take semaphore → Semaphore available
3. HighTask acquires semaphore → executes quickly → releases semaphore
4. NormalTask runs → acquires semaphore → holds it waiting for button
5. HighTask runs again → finds semaphore unavailable → enters Blocked state
6. LowTask runs periodically (doesn't use semaphore)
7. User presses button → NormalTask releases semaphore
8. HighTask (highest priority waiting) immediately acquires semaphore
9. HighTask releases semaphore → NormalTask can acquire again
10. Cycle repeats
```

## UART Output Sequence
```
Entered HighTask and waiting for Semaphore
Semaphore acquired by High Task
Leaving HighTask and releasing Semaphore
Entered NormalTask and waiting for semaphore
Semaphore acquired by Normal Task
(Waiting for button press...)
Entered HighTask and waiting for Semaphore
(Blocked - waiting for semaphore)
(Button Pressed)
Leaving NormalTask and releasing Semaphore
Semaphore acquired by High Task
Leaving HighTask and releasing Semaphore
Entered LowTask
Leaving LowTask
Entered NormalTask and waiting for semaphore
Semaphore acquired by Normal Task
... (cycle repeats)
```

## Hardware Components
- STM32 development board (blue pill)
- USB-to-UART converter (for viewing debug messages)
- Push button connected to PA1

## Pin Configuration
- PA1: Input pin with button (active low when pressed)
- USART1: TX/RX for debug messages (PA9/PA10 on STM32F103)

## Contact
**Rubin Khadka Chhetri**  
📧 rubinkhadka84@gmail.com <br>
🐙 GitHub: https://github.com/rubin-khadka

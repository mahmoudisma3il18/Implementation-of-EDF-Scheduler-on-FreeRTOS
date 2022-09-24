# Implementation-of-EDF-Scheduler-on-FreeRTOS
This Project will discuss how to implement EDF Scheduler on FreeRTOS

In this Project we will change 3 files (task.c , task.h and Freertosconfig.h)

1-First of all, the new Ready List is declared: xReadyTasksListEDF is a simple list structure.
Check Line *378*  in task.c


2-Then, the prvInitialiseTaskLists() method, that initialize all the task lists at the creation of
the first task, is modified adding the initialization of xReadyTasksListEDF.
Check Line *3855*  in task.c


3-prvAddTaskToReadyList() method that adds a task to the Ready List is then modified.
Check Line *221*  in task.c


4-A new variable is added in the tskTaskControlBlock structure (TCB)
Check Line *352*  in task.c


5-A new Initialization task method is created. xTaskPeriodicCreate() is a modified version of the standard method xTaskCreate() , that receives the task
period as additional input parameter and set the xTaskPeriod variable in the task TCB structure. Before adding the new task to the Ready List by calling prvAddTaskToReadyList(), thetask’s xStateListItem is initialized to the value of the next task deadline.
Dont forget to define this function in task.h
Check Line *866*  in task.c


6-vTaskStartScheduler() method initializes the IDLE task and inserts it into the Ready List.
Check Line *2182*  in task.c


7-  Every time the running task is suspended, or a suspended task with an higher priority than the running task awakes, a switch context occurs. vTaskSwitchContext() method is in charge to update the ∗pxCurrentTCB
Check Line *3253*  in task.c


8- Modify the "prvIdleTask" function idle task to keep it always the farest deadline
Check Line *3651*  in task.c


9-In every tick increment, calculate the new task deadline and insert it in the correct position in the EDF ready list In the "xTaskIncrementTick" function.
Check Line *2998*  in task.c

10-In the ""xTaskIncrementTick"" function Make sure that as soon as a new task is available in the EDF ready list, a context switching should take place. Modify preemption way as any task with sooner deadline must preempt task with larger deadline instead of priority"
Check Line *3004*  in task.c

11- add configUSE_EDF_SCHEDULER in freertosconfig.h





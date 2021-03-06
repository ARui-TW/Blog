---
title: "[OS] Chapter 7 â Deadlocks"
date: 2022-01-31 18:07:24
tags:
    - Operating System
    - Notes
categories: Operating System Notes
---
# Deadlock Characterization

{% note default %}
[Notion å¥½è®ç](https://www.notion.so/arui-tw/Chapter-7-Deadlocks-b345b44b6ac14b28a37bdc7bb497edb9)
{% endnote %}

<aside>
ð ç¨å¼ä¹éå½¼æ­¤ç­å¾ï¼ç¡æ³é²è¡äº

</aside>

- Example
    - 2 processes and 2 tape drivers
        - ä¸äººåæ¿ä¸åï¼ç¶å¾ç­å°æ¹æä¸é£å
    - 2 processes, and semaphores A & B
        - P1 (hold B, wait A): wait(A), signal(B)
        - P2 (hold A, wait B): wait(B) , signal(A)

**Necessary Condition** â ä¸å®è¦**å¨é¨**çæ¢ä»¶**åæ**ç¼ç

- Mutual exclusion â è³æºæ¯æ²æå±äº«æ§çï¼æ«æï¼
- Hold and Wait â æ hold resource ç¶å¾ wait å¥äºº
- No preemptive â ä¸è½å¼·å¶å¥äººæ¾é resource
- Circular wait â ä¸å®ææä¸å circular ç waitã
    
    $P_0 \rightarrow P_1  \rightarrow P_1 \rightarrow \dots\rightarrow P_n \rightarrow P_0$
    

![](/assets/[OS]Chapter-7/Untitled.png)

<!-- more -->

# System Model

- è¦ææäºè³æº $R_1, R_2, \dots, R_m$
    - E.g. CPU, memory pages, I/O devices
- æ¯åè³æº $R_i$ å¯è½ææè¤æ¸çå± $W_i$ å instances
    - E.g. a computer has 2 CPUs
- æ¯ä¸å process åå¾ resource çæ¹å¼ï¼
    
    Request â use â release
    

## Resource-Allocation Graph

![åå½¢è·æ­£æ¹å½¢ä¸è½æ¹](/assets/[OS]Chapter-7/Untitled%201.png)

- 3 processes, P1 *~ P3*
- 4 resources, R1 *~ R4*
    - R1 and R3 each has one instance
    - R2 has two instances
    - R4 has three instances
- Request edges:
    - P1 â R1: P1 requests R1 ï¼ä¸ç´å­å¨å çº resource æ­£å¨è¢«ç¨ï¼
- Assignment edges:
    - R2 â P1: é£å instance æ­£å¨è¢« P2 ä½¿ç¨

â P1 is hold on an instance of R2 and waiting for an instance of R1

- If the graph contains a cycle, a deadlock may exist
    - å¦ææ multiple instancesï¼é£å°±è¦æª¢æ¥æ¯ä¸æ¢è·¯ç·é½æ¯ cycle æè¡
    - Example 1
        
        ![](/assets/[OS]Chapter-7/Untitled%202.png)
        
        1. P1 is waiting for P2
        2. P2 is waiting for P3 â P1 is waiting for P3
        3. P3 is waiting for P1 or P2, and they both waiting for P3
        
        â deadlock!
        
    - Example 2
        
        ![](/assets/[OS]Chapter-7/Untitled%203.png)
        
        1. P1 is waiting for P2 or P3
        2. P3 is waiting for P1 or P4
        3. Since P2 and P4 wait no one
        
        â no deadlock

### Deadlock Detection

- æ² cycle â æ² deadlock
- æ cycle
    - one instance per resource type â æ deadlock
    - multiple instances per resource type â å¯è½æ deadlock

### Handling Deadlocks

- ç¢ºä¿ä¸æé²å»
    - deadlock prevention â ç¢ºä¿è³å°ä¸åå¿è¦æ¢ä»¶ä¸æç¼ç
    - deadlock avoidance â å¨ runtime å»ä¸æ·çæª¢æ¥
- è®ä»é²å»å recover
    - deadlock detection
    - deadlock recovery
- å®å¨ä¸èçï¼ignore the problemï¼
    - è® user å»ä¸­æ·ä»

## Review Slides ( I )

- deadlock necessary conditions?
    - mutual exclusion
    - hold & wait
    - no preemption
    - circular wait
- resource-allocation graph?
    - cycle in RAG â deadlock?
- deadlock handling types?
    - deadlock prevention
    - deadlock avoidance
    - deadlock recovery
    - ignore the problem


# Deadlock Prevention & Deadlock Avoidance

## Deadlock Prevention

- æ¯ä¸ç¨® resource åæ¿æä¸ç¨®æ¢ä»¶ãï¼æ¯ä¸åå¯ä»¥ä¸ä¸æ¨£ï¼
    - Mutual exclusion (ME): do not require ME on sharable resources
        - E.g. set file to read only â no mutual exclusion (can many read in same time)
        - ä½ä¸æ¯ææé½å¯ä»¥éæ¨£è§£
        - ï¼Memory æ¯ support concurrent access çï¼æ¯ programer çè¡çºè®ä»æ ME çæ¢ä»¶åºä¾çï¼
    - Hold & Wait
        - è¦å®ä»è¦ free ææä¸ç resource æè½å»è¦å¶ä» resourceãææ¯ä¸å°±è¦æå¨é¨ç resource è¦é½ï¼åªè¦è¦ç­å°±è¦ææ¿å°ç free æï¼å¯ä»¥ç­ï¼ä½æ¯æä¸ä¸è½ææ±è¥¿ï¼ã
        - All or nothing allocation
        - å»æ¹è® mutex_wait çå¯¦ä½
        
        â¹ï¸Â Resource ä½¿ç¨çéä½ï¼å¯è½ææ starvation
        
    - No preemption
        - æ¹æå¯ä»¥ä½ preemptive
        - E.g. preemptive ç scheduler â æ timer å»è® OS always control CPU
        
        â¹ï¸Â Overheadï¼è¦å¤ä¸å¡ memory å»æ«å­è³æ
        
        â Printer çä»£å¹å°±å¤ªé«äºï¼è¦éå°ï¼
        
    - Circular wait
        - å¨ request resource çæåï¼å¦æä½ è¦ request $R_k$ï¼é£å°±è¦ release $R_i, \forall i \geq k$ãï¼å°æ¼ $k$ çå¯ä»¥ç¹¼çº holdï¼
            
            æä¸ä¸è½æ¡æ¯è¼å¤§çã
            
        - Example
            - F(*tape drive*) = 1, F(*disk drive*) = 5, F(*printer*) = 12
            - å¦æ hold diskï¼å¯ä»¥è¦ printer
            - å¦æ hold printer å tapeï¼é£ request drive å°±è¦ release printerï¼tape å¯ä»¥çè
        - Proof
            - è­ææ deadlock ä½æ¯ä¸å¯è½æç«
            - $P_0(R_0) \rightarrow R_1$($P_0$ hold $R_0$ and request $R_1$)
                
                $P_1(R_1) \rightarrow R_2, \dots, P_N(R_N) \rightarrow \red {R_0}$
                
            - ä½æ¯æå¾ä¸åä¸å¯è½æç«ï¼å çº $N>0$

## Avoidance Algorithms

- Single instance of a resource type
    
    â ç¨ resource-allocation graph (RAG) algorithm ****ç´æ¥åµæ¸¬ circle
    
- Multiple instances of a resource type
    
    â bankerâs algorithm
    

### Resource-Allocation Graph (RAG) Algorithm

![](/assets/[OS]Chapter-7/Untitled%204.png)

- Request edges:
    - $P_i \rightarrow R_j$: $P_i$ waiting for $R_j$
- Assignment edges:
    - $R_j â P_i$: $R_j$ is allocated and held by $P_i$
    - Resource è¢« release å¯è½è½å claim edge
- Claim edge:
    - $P_i \rightarrow R_j$: $P_i$ may request $R_j$ **in the future** (worst case)
    - ççç¼çå°±æè®æ request edges
- Example
    - éä¸æåºäº
        
        ![](/assets/[OS]Chapter-7/Untitled%205.png)
        
    - ç¶ P2 request for R1 çæåï¼ä»æè¢«ææãå çºæåæåè¨­ claim çæå½¢æå¨æªä¾ somehow ç¼çã
        
        ![](/assets/[OS]Chapter-7/Untitled%206.png)
        
- Cycle-detection algorithm â $O(n^2)$

### **Bankerâs algorithm**

å»èæ® worse caseã

![](/assets/[OS]Chapter-7/Untitled%207.png)

- Safe state â å¯ä»¥æ¾å°ä¸å safe sequence è®ç¨å¼ç§èè·ï¼çµæåé½ä¸æéå° deadlock
- Unsafe state â ææ©çæ deadlock
- Deadlock avoidance â ä¸è¦é²å° unsafe

**Safe Sequence**

- Assume æ 12 å tape drives
- Safe state:
    - At t0:
        
        
        |  | Max Needs | Current Holding |
        | --- | --- | --- |
        | P0 | 10 | 5 |
        | P1 | 4 | 2 |
        | P2 | 9 | 2 |
        - Max need is hinted from processes
        - Max need is worst case
        
        â <P1, P0, P2> is a safe sequence
        
        1. çµ¦ P1 3 åï¼æçé½çµ¦ä»ï¼
            
            
            |  | Max Needs | Current Holding | Available |
            | --- | --- | --- | --- |
            | P0 | 10 | 5 |  |
            | P1 | 4 | 2 | 3 |
            | P2 | 9 | 2 |  |
        2. çµ¦ P0 5 å
            
            
            |  | Max Needs | Current Holding | Available |
            | --- | --- | --- | --- |
            | P0 | 10 | 5 | 5 |
            | P1 | 4 | 0 | 0 |
            | P2 | 9 | 2 |  |
        3. çµ¦ P2 10 å
            
            
            |  | Max Needs | Current Holding | Available |
            | --- | --- | --- | --- |
            | P0 | 10 | 0 |  |
            | P1 | 4 | 0 |  |
            | P2 | 9 | 2 | 10 |
- Unsafe state:
    - At t1:
        
        åè¨­éæå OS æ¿å°çä¸å request èªª P2 æ³è¦å¤ä¸å tape driveã
        
        |  | Max Needs | Current Holding | Available |
        | --- | --- | --- | --- |
        | P0 | 10 | 5 |  |
        | P1 | 4 | 2 | 3 â 2 |
        | P2 | 9 | 2 â 3 |  |
        - è·ä¸æ¬¡ä¸é¢çæµç¨ï¼ç¶å¾æåå°±æç¼ç¾ï¼å¦ææååæäºï¼P2 ç current holding è®æ 3 äºï¼ï¼æåå°±æç¼ç¾ä»å¯è½æ deadlock ï¼i.e. é²å° unsafe stateï¼ã
        - æä»¥ OS å°±æææéå requestã
- Banker algorithm:
    1. æ¿å° requestï¼ååè¨­åæäº
    2. ç¶å¾å»è· safe sequence
        
        é¸ sequence çé åºé¨ä¾¿é½æ²å·®ã
        
    3. å¦æ safe sequence å­å¨ï¼æåå°± grant requestï¼åä¹å°±ä¸åæ
        
        å¦æææ¸åï¼é£å°±é½å¯ä»¥ï¼åæ­£è³å°ä¸åå°±å¥½äº
        
- Bankerâs Algorithm Example 1
    
    ææ¶å¾è²¼.. P.27
    
- Programming Example
    
    A, B, C are semaphores initialized with the values of 2, 1, 1.
    
    ç¨ round-robin è·ã
    
    - åæ¬
        
        ![](/assets/[OS]Chapter-7/Untitled%208.png)
        
    - Use avoidance
        
        ![](/assets/[OS]Chapter-7/Untitled%209.png)
        
        - P4 ç wait(A) å¦ææåäºï¼å ä¸ claim edge å°±æ dead lockï¼æä»¥ OS ä¸æè®ä»æ¿
        - æ¥ä¸ä¾ P2 å°±ææ¶å° Aï¼ç¶å¾ eventually ä»å°±æ signal A and Cï¼å©ä¸çå°±ææåçµæäº

## Review Slides (II)

- deadlock prevention methods?
    - mutual exclusion
    - hold & wait
    - no preemption
    - circular wait
- deadlock avoidance methods?
    - safe state definition?
    - safe sequence?
    - claim edge?

# Deadlock Detection & Recovery from Deadlock

- deadlock äºåè§£
    
    ï¼å°±ç®å¡ä½äºï¼ä¹åªæ¯ user programï¼OS ä¸å®ä¸æå¡ä½çå¦ï¼è¦ä¿¡ä»» OSï¼
    
- æä»¥å°±å°±ç¶ä¸çææ³å»è·ä¸é¢é£äº check
- Single instance
    
    ![](/assets/[OS]Chapter-7/Untitled%2010.png)
    
- Multiple-Instance â Bankerâs algorithm
    - å¯ä»¥æ Max Needs æ¿æï¼æ²ç¨äº
    - å»æª¢æ¥ææ²æ safe sequence å­å¨ï¼æå°±ç¶ä½æ²äºãæ²æå°±ä»£è¡¨é²å¥ deadlock äºã
    
    ![](/assets/[OS]Chapter-7/Untitled%2011.png)
    
- Deadlock Recovery
    
    â Process termination
    
    - ä½æ¯è¦ kill èª°ï¼ï¼å¯è½å¯ä»¥ kill priority ä½çï¼ï¼ä½æ¯ä¸ç´ kill priority ä½çå¯è½æ starvation ï¼
    - Kill æä¹å¾éå¯ä»¥ roll back åï¼

# Textbook Problem Set

![](/assets/[OS]Chapter-7/Untitled%2012.png)

![](/assets/[OS]Chapter-7/Untitled%2013.png)

![](/assets/[OS]Chapter-7/Untitled%2014.png)

![](/assets/[OS]Chapter-7/Untitled%2015.png)

{% note warning %}  
æ­¤ç­è¨çºæ¸è¯å¤§å­¸å¨å¿é ææä½æ¥­ç³»çµ±ä¹èª²å ç­è¨ï¼ææå§å®¹ååççåææ¼èª²å å§å®¹ã<br />
å¦å§å®¹æèª¤ï¼æ­¡è¿ä¾ä¿¡ [mail@arui.dev](mailto:mail@arui.dev)ã
{% endnote %}
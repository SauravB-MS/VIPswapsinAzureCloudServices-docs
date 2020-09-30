---
title: Operating system performance degrades
description: Provides a solution to an issue that operating system performance may degrade when one or more processes access multiple large files using the CreateFile() API and the FILE_FLAG_RANDOM_ACCESS flag.
ms.date: 09/23/2020
author: Deland-Han 
ms.author: delhan
manager: dscontentpm
audience: itpro
ms.topic: troubleshooting
ms.prod: windows-server
localization_priority: medium
ms.reviewer: kaushika, raackley
ms.prod-support-area-path: COM and DCOM programming
ms.technology: ApplicationCompatibility
---
# Performance degrades when accessing large files with FILE_FLAG_RANDOM_ACCESS

This article provides a solution to an issue that operating system performance may degrade when one or more processes access multiple large files using the CreateFile() API and the FILE_FLAG_RANDOM_ACCESS flag.

_Original product version:_ &nbsp; Windows Server 2012 R2  
_Original KB number:_ &nbsp; 2549369

## Symptoms

Operating system performance may degrade when one or more processes access multiple large files using the CreateFile() API and the FILE_FLAG_RANDOM_ACCESS flag. The performance degradation is because of the system cache consuming available memory (visible in the performance counter Memory\Cache Bytes).

## Cause

The FILE_FLAG_RANDOM_ACCESS flag is a hint to the Cache Manager that the file will be opened for random I/O. Random I/O means there is no predictable pattern to the I/O that will take place. This flag disables intelligent read-aheads and prevents automatic unmapping of views after pages are read to minimize mapping/unmapping when the process revisits that part of the file. This keeps previously read views in the Cache Manager working set. However, if the cumulative size of the accessed files exceeds physical memory, keeping so many views in the Cache Manager working set may be detrimental to overall operating system performance because it can consume a large amount of physical RAM.

## Resolution

Remove the FILE_FLAG_RANDOM_ACCESS flag when the application opens the file(s) with CreateFile. This will allows the views to be unmapped and moved to the standby list after page reads are completed. This will require involvement from the software developer.

## More information

CreateFile Function  
[https://msdn.microsoft.com/library/aa363858(v=VS.85).aspx](https://msdn.microsoft.com/library/aa363858%28v=vs.85%29.aspx)
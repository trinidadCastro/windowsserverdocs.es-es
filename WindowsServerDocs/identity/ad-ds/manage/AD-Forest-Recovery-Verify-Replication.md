---
title: "Bosque de AD recuperación - comprobar la replicación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 302e522a-fb40-43bc-bc63-83dcc87ebde5
ms.technology: identity-adfs
ms.openlocfilehash: d85336a10e808755677a99777af8ecf5c07b05ab
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="resources-to-verify-replication-is-working"></a>Recursos para comprobar la replicación funciona 

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2
 
 Después de restaurar o volver a instalar todos los controladores de dominio, puede comprobar que AD DS y SYSVOL son recuperada y reproducen correctamente mediante **/Replsum repadmin**, que se ejecuta en cualquier versión de Windows Server.  
  
> [!TIP]
>  También puedes descargar y ejecutar la [herramienta de estado de replicación de Active Directory](https://www.microsoft.com/download/details.aspx?id=30005) (ADReplStatus), una herramienta gratuita que supervisa el estado de replicación de los controladores de dominio y los informes de errores. ADReplStatus requiere .NET Framework 4, que se instalará si aún no está presente.  
  
 Comprueba el registro de replicación DFS en el Visor de eventos para 4602 de identificador de evento (o identificador de evento de servicio de replicación 13516), que indica que se ha inicializado SYSVOL.  
  
 Si la primera recuperado DC registra 4614 de identificador de evento ("el controlador de dominio está esperando para realizar la replicación inicial. La carpeta replicada permanecerá en el estado de sincronización inicial hasta que se replique con su partner") en el registro de replicación de DFS, a continuación, el identificador de evento 4602 no aparece y necesitas realizar los siguientes pasos manuales para recuperar SYSVOL si se replica por DFSR:  
  
1.  Cuando DFSR evento 4612 aparece en el primer controlador de dominio restaurado realizar una restauración manual como se describe en [2218556: cómo forzar una sincronización autorizada y no autorizados para replicar DFSR SYSVOL (por ejemplo, "D4/D2" para FRS)](https://support.microsoft.com/kb/2218556) (https://support.microsoft.com/kb/2218556).  
  
2.  Establecer **SysvolReady marca** en 1 manualmente, como se describe en [recurso compartido NETLOGON el 947022 no está presente después de instalar los servicios de dominio de Active Directory en un nuevo controlador de dominio completo o de sólo lectura basado en Windows Server 2008](https://support.microsoft.com/kb/947022).  
  
 También puedes crear un informe de diagnóstico de replicación DFS. Para obtener más información, consulta [crear un informe de diagnóstico para la replicación de DFS](https://technet.microsoft.com/library/cc754227.aspx) y [DFS Step-by-Step Guide for Windows Server 2008](https://technet.microsoft.com/library/cc732863\(WS.10\).aspx). Si el servidor ejecuta Windows Server 2008 R2, puedes usar [modificador de línea de comandos de dfsrdiag.exe ReplicationState](http://blogs.technet.com/b/filecab/archive/2009/05/28/dfsrdiag-exe-replicationstate-what-s-dfsr-up-to.aspx).  
  
 También puedes ejecutar la prueba de replicación mediante dcdiag.exe para comprobar si hay errores de replicación. Para obtener más información, consulta el tema de Knowledge Base [artículo 249256](https://support.microsoft.com/kb/249256).

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosque de AD - procedimientos](AD-Forest-Recovery-Procedures.md)

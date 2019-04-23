---
title: 'Bosque de AD recuperación: comprobar la replicación'
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 302e522a-fb40-43bc-bc63-83dcc87ebde5
ms.technology: identity-adds
ms.openlocfilehash: fb05586f281460dc2c7a1afea4c0423493e3fc46
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878316"
---
# <a name="resources-to-verify-replication-is-working"></a>Recursos para comprobar que funciona la replicación 

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Después de restaurar o volver a instalar todos los controladores de dominio, puede comprobar que AD DS y SYSVOL están replicando y recuperados correctamente mediante el uso de **repadmin/Replsum**, que se ejecuta en cualquier versión de Windows Server.  
  
> [!TIP]
> También puede descargar y ejecutar la [herramienta de estado de replicación de Active Directory](https://www.microsoft.com/download/details.aspx?id=30005) (ADReplStatus), una herramienta gratuita que supervisa el estado de replicación de los controladores de dominio y notifica los errores. ADReplStatus requiere .NET Framework 4, que se instalará si aún no está presente.  

Compruebe el registro de la replicación DFS en el Visor de eventos para 4602 de Id. de evento (o Id. de evento de servicio de replicación de archivos 13516), lo que indica que se inicializó SYSVOL.  

Si recupera el primer controlador de dominio registra 4614 de Id. de evento ("el controlador de dominio está esperando para realizar la replicación inicial. La carpeta replicada permanecerá en el estado de sincronización inicial hasta que se replique con su partner") en el registro de la replicación DFS, a continuación, 4602 de Id. de evento no aparece y se debe realizar los siguientes pasos manuales para recuperar SYSVOL si se replican de forma DFSR:  

1. Cuando DFSR eventos 4612 aparece en el primer controlador de dominio restaurado realizar una restauración autoritativa manual como se describe en [2218556: Cómo forzar una sincronización autoritativa y no autoritativa para SYSVOL replicado mediante DFSR (como "D4/D2" para FRS)](https://support.microsoft.com/kb/2218556) (https://support.microsoft.com/kb/2218556).  
2. Establecer **SysvolReady marca** 1 manualmente, tal como se describe en [947022 share The NETLOGON no está presente después de instalar servicios de dominio de Active Directory en un nuevo controlador de dominio completo o solo lectura basado en Windows Server 2008 ](https://support.microsoft.com/kb/947022).  

También puede crear un informe de diagnóstico de replicación DFS. Para obtener más información, consulte [crear un informe de diagnóstico para la replicación DFS](https://technet.microsoft.com/library/cc754227.aspx) y [Guía de paso a paso de DFS para Windows Server 2008](https://technet.microsoft.com/library/cc732863\(WS.10\).aspx). Si el servidor se está ejecutando Windows Server 2008 R2, puede usar [modificador de línea de comandos dfsrdiag.exe ReplicationState](http://blogs.technet.com/b/filecab/archive/2009/05/28/dfsrdiag-exe-replicationstate-what-s-dfsr-up-to.aspx).  

También puede ejecutar la prueba de replicaciones usando dcdiag.exe para comprobar los errores de replicación. Para obtener más información, vea Knowledge Base [artículo 249256](https://support.microsoft.com/kb/249256).

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación de bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosques de AD: procedimientos](AD-Forest-Recovery-Procedures.md)

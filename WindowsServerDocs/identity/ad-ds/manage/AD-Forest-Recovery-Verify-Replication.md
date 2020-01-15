---
title: 'Recuperación de bosque de AD: comprobar la replicación'
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 302e522a-fb40-43bc-bc63-83dcc87ebde5
ms.technology: identity-adds
ms.openlocfilehash: f6bee5164849d6643c1744ce121b9ce91b5e7f7f
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949040"
---
# <a name="resources-to-verify-replication-is-working"></a>Recursos para comprobar que la replicación funciona 

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Después de restaurar o volver a instalar todos los controladores de sesión, puede comprobar que AD DS y SYSVOL se recuperan y se replican correctamente mediante **repadmin/replsum**, que se ejecuta en cualquier versión de Windows Server.  
  
> [!TIP]
> También puede descargar y ejecutar la [herramienta de Active Directory Replication status](https://www.microsoft.com/download/details.aspx?id=30005) (ADReplStatus), una herramienta gratuita que supervisa el estado de replicación de los controladores de servicio y los errores de informes. ADReplStatus requiere .NET Framework 4, que se instalará si aún no está presente.  

Compruebe el registro de Replicación DFS en Visor de eventos para el ID. de evento 4602 (o el ID. de evento de servicio de replicación de archivos 13516), que indica que se ha inicializado SYSVOL.  

Si el primer registro recuperado registra el ID. de evento 4614 ("el controlador de dominio está esperando para realizar la replicación inicial. La carpeta replicada permanecerá en el estado de sincronización inicial hasta que se haya replicado con su asociado ") en el registro de Replicación DFS, entonces el ID. de evento 4602 no aparece y debe realizar los siguientes pasos manuales para recuperar SYSVOL si lo Replica. DFSR  

1. Cuando aparece el evento 4612 de DFSR en el primer controlador de dominio restaurado, realice una restauración autoritativa manualmente como se describe en [2218556: Cómo forzar una sincronización autoritativa y no autoritativa para SYSVOL replicado con DFSR (como "D4/D2" para FRS)](https://support.microsoft.com/kb/2218556) (https://support.microsoft.com/kb/2218556).  
2. Establezca la **marca SysvolReady** en 1 manualmente, tal y como se describe en [947022 el recurso compartido Netlogon no está presente después de instalar Active Directory Domain Services en un nuevo controlador de dominio basado en Windows Server 2008 completo o de solo lectura](https://support.microsoft.com/kb/947022).  

También puede crear un informe de diagnóstico Replicación DFS. Para obtener más información, vea [crear un informe de diagnóstico para replicación DFS](https://technet.microsoft.com/library/cc754227.aspx) y la [Guía paso a paso de DFS para Windows Server 2008](https://technet.microsoft.com/library/cc732863\(WS.10\).aspx). Si el servidor ejecuta Windows Server 2008 R2, puede usar el [modificador de la línea de comandos de Dfsrdiag. exe ReplicationState](https://blogs.technet.com/b/filecab/archive/2009/05/28/dfsrdiag-exe-replicationstate-what-s-dfsr-up-to.aspx).  

También puede ejecutar la prueba de replicación con DCDiag. exe para comprobar si hay errores de replicación. Para obtener más información, vea el [artículo 249256](https://support.microsoft.com/kb/249256)de Knowledge base.

## <a name="next-steps"></a>Pasos a seguir

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)

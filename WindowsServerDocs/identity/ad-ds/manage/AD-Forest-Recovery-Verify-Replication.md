---
title: 'Recuperación de bosque de AD: comprobar la replicación'
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 302e522a-fb40-43bc-bc63-83dcc87ebde5
ms.technology: identity-adds
ms.openlocfilehash: c44f7abd14a65178b84194f43dad829df81fa77b
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86960837"
---
# <a name="resources-to-verify-replication-is-working"></a>Recursos para comprobar que la replicación funciona 

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Después de restaurar o volver a instalar todos los controladores de sesión, puede comprobar que AD DS y SYSVOL se recuperan y se replican correctamente mediante **repadmin/replsum**, que se ejecuta en cualquier versión de Windows Server.  
  
> [!TIP]
> También puede descargar y ejecutar la [herramienta de Active Directory Replication status](https://www.microsoft.com/download/details.aspx?id=30005) (ADReplStatus), una herramienta gratuita que supervisa el estado de replicación de los controladores de servicio y los errores de informes. ADReplStatus requiere .NET Framework 4, que se instalará si aún no está presente.  

Compruebe el registro de Replicación DFS en Visor de eventos para el ID. de evento 4602 (o el ID. de evento de servicio de replicación de archivos 13516), que indica que se ha inicializado SYSVOL.  

Si el primer registro recuperado registra el ID. de evento 4614 ("el controlador de dominio está esperando para realizar la replicación inicial. La carpeta replicada permanecerá en el estado de sincronización inicial hasta que se haya replicado con su asociado ") en el registro de Replicación DFS, entonces el ID. de evento 4602 no aparece y debe realizar los siguientes pasos manuales para recuperar SYSVOL en caso de que DFSR lo replique:  

1. Cuando aparece el evento 4612 de DFSR en el primer controlador de dominio restaurado, realice una restauración autoritativa manualmente como se describe en [2218556: Cómo forzar una sincronización autoritativa y no autoritativa para SYSVOL replicado con DFSR (como "D4/D2" para FRS)](https://support.microsoft.com/kb/2218556) ( https://support.microsoft.com/kb/2218556) .  
2. Establezca la **marca SysvolReady** en 1 manualmente, tal y como se describe en [947022 el recurso compartido Netlogon no está presente después de instalar Active Directory Domain Services en un nuevo controlador de dominio basado en Windows Server 2008 completo o de solo lectura](https://support.microsoft.com/kb/947022).  

También puede crear un informe de diagnóstico Replicación DFS. Para obtener más información, vea [crear un informe de diagnóstico para replicación DFS](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754227(v=ws.11)) y la [Guía paso a paso de DFS para Windows Server 2008](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754227(v=ws.11)). Si el servidor ejecuta Windows Server 2008 R2, puede usar [dfsrdiag.exe modificador](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754227(v=ws.11))de la línea de comandos de ReplicationState.  

También puede ejecutar la prueba de replicaciones mediante dcdiag.exe para comprobar si hay errores de replicación. Para obtener más información, vea el [artículo 249256](https://support.microsoft.com/kb/249256)de Knowledge base.

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)

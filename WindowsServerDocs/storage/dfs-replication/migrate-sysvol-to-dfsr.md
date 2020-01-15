---
title: Migrar la replicación de SYSVOL a la replicación de DFS
ms.date: 07/02/2012
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: d8d9d47ff8f14ce316d2352729247ab2dcf4acbc
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949701"
---
# <a name="migrate-sysvol-replication-to-dfs-replication"></a>Migrar la replicación de SYSVOL a la replicación de DFS


Actualizado: 25 de agosto de 2010

Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Los controladores de dominio usan una carpeta compartida especial llamada SYSVOL para replicar los scripts de inicio de sesión y los archivos de directiva de grupo objeto en otros controladores de dominio. Windows 2000 Server y Windows Server 2003 usan el servicio de replicación de archivos (FRS) para replicar SYSVOL, mientras que Windows Server 2008 usa el servicio de Replicación DFS más reciente en los dominios que usan el nivel funcional de dominio de Windows Server 2008 y FRS para los dominios que ejecutar niveles funcionales de dominio anteriores.

Para usar Replicación DFS para replicar la carpeta SYSVOL, puede crear un nuevo dominio que use el nivel funcional de dominio de Windows Server 2008, o bien puede usar el procedimiento que se describe en este documento para actualizar un dominio existente y migrar la replicación a Replicación DFS.

En este documento se supone que tiene un conocimiento básico de Active Directory Domain Services (AD DS), FRS y la replicación de Sistema de archivos distribuido (Replicación DFS). Para obtener más información, vea información general de [Active Directory Domain Services](https://go.microsoft.com/fwlink/?linkid=147787), [información general de FRS](https://go.microsoft.com/fwlink/?linkid=121763)o [información general de replicación DFS](https://go.microsoft.com/fwlink/?linkid=121762)


> [!NOTE]
> Para descargar una versión imprimible de esta guía, vaya a la <a href="https://go.microsoft.com/fwlink/?linkid=150375">Guía de migración de replicación de SYSVOL: FRS a replicación DFS</a> (https://go.microsoft.com/fwlink/?LinkId=150375)
<br>


## <a name="in-this-guide"></a>En esta guía

[Información conceptual sobre la migración de SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640170(v=ws.10))

  - [Estados de migración de SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641052(v=ws.10))  
      
  - [Información general sobre el procedimiento de migración de SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639809(v=ws.10))  
      

[Procedimiento de migración de SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639860(v=ws.10))

  - [Migrar al estado preparado](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641193(v=ws.10))  
      
  - [Migrar al estado Redirigido](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641340(v=ws.10))  
      
  - [Migrar al estado eliminado](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640254(v=ws.10))  
      

[Solucionar problemas de migración de SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640395(v=ws.10))

  - [Solucionar problemas de migración de SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639976(v=ws.10))  
      
  - [Revertir la migración de SYSVOL a un estado estable anterior](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640509(v=ws.10))  
      

[Información de referencia de migración de SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640293(v=ws.10))

  - [Escenarios de migración de SYSVOL admitidos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639854(v=ws.10))  
      
  - [Comprobar el estado de la migración de SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639789(v=ws.10))  
      
  - [Dfsrmig](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641227(v=ws.10))  
      
  - [Acciones de la herramienta de migración de SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639712(v=ws.10))  
      

## <a name="additional-references"></a>Referencias adicionales

[Serie de migración de SYSVOL: parte 1: Introducción al proceso de migración de SYSVOL](https://go.microsoft.com/fwlink/?linkid=121756)

[Serie de migración de SYSVOL: parte 2: Dfsrmig. exe: la herramienta de migración de SYSVOL](https://go.microsoft.com/fwlink/?linkid=121757)

[Serie de migración de SYSVOL: parte 3: migrar al estado "preparado"](https://go.microsoft.com/fwlink/?linkid=121758)

[Serie de migración de SYSVOL: parte 4: migrar al estado "REDIRIGIDO"](https://go.microsoft.com/fwlink/?linkid=121759)

[Serie de migración de SYSVOL: parte 5: migrar al estado ' eliminado '](https://go.microsoft.com/fwlink/?linkid=121760)

[Guía paso a paso para sistemas de archivos distribuidos en Windows Server 2008](https://go.microsoft.com/fwlink/?linkid=85231)

[Referencia técnica de FRS](https://go.microsoft.com/fwlink/?linkid=121764)


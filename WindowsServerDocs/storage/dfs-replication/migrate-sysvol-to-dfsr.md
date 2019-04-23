---
Title: Migrar la replicación de SYSVOL a la replicación de DFS
ms.date: 07/02/2012
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 7ea883730fcab83d064fa41f610bde4837510276
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836646"
---
# <a name="migrate-sysvol-replication-to-dfs-replication"></a>Migrar la replicación de SYSVOL a la replicación de DFS


Actualizado: 25 de agosto de 2010

Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Controladores de dominio usan una carpeta compartida especial denominada SYSVOL para replicar scripts de inicio de sesión y los archivos de objeto de directiva de grupo a otros controladores de dominio. Windows 2000 Server y Windows Server 2003 utilizan el servicio de replicación de archivos (FRS) para replicar SYSVOL, mientras que Windows Server 2008 usa el servicio de replicación DFS más reciente en dominios que usen el nivel funcional del dominio de Windows Server 2008 y FRS para dominios que ejecute los niveles funcionales de dominio anteriores.

Para usar la replicación DFS para replicar la carpeta SYSVOL, puede crear un nuevo dominio que usa el nivel funcional del dominio de Windows Server 2008, o puede usar el procedimiento que se describe en este documento para actualizar un dominio existente y migrar a la replicación Replicación DFS.

Este documento se da por supuesto que tiene conocimientos básicos de los servicios de dominio de Active Directory (AD DS), FRS y replicación de sistema de archivos distribuido (replicación DFS). Para obtener más información, consulte [Introducción a servicios de dominio de Active Directory](http://go.microsoft.com/fwlink/?linkid=147787), [Introducción a FRS](http://go.microsoft.com/fwlink/?linkid=121763), o [información general de la replicación DFS](http://go.microsoft.com/fwlink/?linkid=121762)


> [!NOTE]
> Para descargar una versión imprimible de esta guía, vaya a <a href="http://go.microsoft.com/fwlink/?linkid=150375">Guía de migración de replicación de SYSVOL: FRS a replicación DFS</a> ()http://go.microsoft.com/fwlink/?LinkId=150375)
<br>


## <a name="in-this-guide"></a>En esta guía

[Información Conceptual de migración SYSVOL](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640170(v=ws.10))

  - [Estados de migración SYSVOL](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641052(v=ws.10))  
      
  - [Información general sobre el procedimiento de migración SYSVOL](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639809(v=ws.10))  
      

[Procedimiento de migración SYSVOL](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639860(v=ws.10))

  - [Migrar al estado preparado](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641193(v=ws.10))  
      
  - [Migrar al estado redirigido](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641340(v=ws.10))  
      
  - [Migrar al estado eliminado](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640254(v=ws.10))  
      

[Solución de problemas de migración SYSVOL](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640395(v=ws.10))

  - [Solucionar problemas de migración SYSVOL](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639976(v=ws.10))  
      
  - [Revertir la migración de SYSVOL a un estado estable anterior](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640509(v=ws.10))  
      

[Información de referencia de migración SYSVOL](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640293(v=ws.10))

  - [Escenarios de migración SYSVOL admitidos](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639854(v=ws.10))  
      
  - [Comprobar el estado de migración SYSVOL](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639789(v=ws.10))  
      
  - [Dfsrmig](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641227(v=ws.10))  
      
  - [Acciones de herramienta de migración SYSVOL](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639712(v=ws.10))  
      

## <a name="additional-references"></a>Referencias adicionales

[Serie de migración de SYSVOL: Parte 1: Introducción al proceso de migración SYSVOL](http://go.microsoft.com/fwlink/?linkid=121756)

[Serie de migración de SYSVOL: La primera parte 2—Dfsrmig.exe: La herramienta de migración SYSVOL](http://go.microsoft.com/fwlink/?linkid=121757)

[Serie de migración de SYSVOL: Parte 3: migrar al estado "Preparado"](http://go.microsoft.com/fwlink/?linkid=121758)

[Serie de migración de SYSVOL: Parte 4: migrar al estado "REDIRECTED"](http://go.microsoft.com/fwlink/?linkid=121759)

[Serie de migración de SYSVOL: Parte 5: migrar al estado 'Eliminado'](http://go.microsoft.com/fwlink/?linkid=121760)

[Guía paso a paso para los sistemas de archivos distribuido en Windows Server 2008](http://go.microsoft.com/fwlink/?linkid=85231)

[Referencia técnica de FRS](http://go.microsoft.com/fwlink/?linkid=121764)


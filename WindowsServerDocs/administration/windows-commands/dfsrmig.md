---
title: dfsrmig
description: Tema de referencia de dfsrmig, que migra la replicación de SYSvol desde el servicio de replicación de archivos (FRS) a la replicación de Sistema de archivos distribuido (DFS), proporciona información sobre el progreso de la migración y modifica los objetos de servicios de dominio de Active Directory (AD DS) para admitir la migración.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e1b6a464-6a93-4e66-9969-04f175226d8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7a5cac54a1ad96994059f3131b81702136b26aad
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719536"
---
# <a name="dfsrmig"></a>dfsrmig

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Migra la replicación de SYSvol desde el servicio de replicación de archivos (FRS) a la replicación de Sistema de archivos distribuido (DFS), proporciona información sobre el progreso de la migración y modifica los objetos de servicios de dominio de Active Directory (AD DS) para admitir la migración.



## <a name="syntax"></a>Sintaxis

```
dfsrmig [/SetGlobalState <state> | /GetGlobalState | /GetMigrationState | /createGlobalObjects | 
/deleteRoNtfrsMember [<read_only_domain_controller_name>] | /deleteRoDfsrMember [<read_only_domain_controller_name>] | /?]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción | | -----_ | ------ | | /SetGlobalState <state> | Establece el estado de migración global deseado para el dominio en el estado que corresponde al valor especificado por *Estado*.<p>Para continuar con la migración o los procesos de reversión, use este comando para recorrer los Estados válidos. Esta opción permite iniciar y controlar el proceso de migración mediante el establecimiento del estado de migración global en AD DS en el emulador de PDC. Si el emulador de PDC no está disponible, se produce un error en este comando.<p>Solo puede establecer el estado de migración global en un estado estable. Los valores válidos para el *Estado*, por lo tanto, son **0** para el estado de inicio, **1** para el estado preparado, **2** para el estado Redirigido y **3** para el estado eliminado.<p>La migración al estado eliminado es irreversible y no es posible revertir desde ese estado, por lo que debe usar un valor de **3** para el *Estado* solo cuando se haya confirmado por completo el uso de replicación DFS para la replicación de SYSvol. | | /GetGlobalState | Recupera el estado de migración global actual para el dominio de la copia local de la base de datos de AD DS, cuando se ejecuta en el emulador de PDC.<p>Use esta opción para confirmar que estableció el estado de migración global correcto. Solo los Estados de migración estables pueden ser Estados de migración globales, por lo que los resultados que el comando **dfsrmig** notifica con la opción **/GetGlobalState** se corresponden con los Estados que se pueden establecer con la opción **/SetGlobalState** .<p>Debe ejecutar el comando **dfsrmig** con la opción **/GetGlobalState** solo en el emulador de PDC. la replicación de Active Directory replica el estado global en los otros controladores de dominio del dominio, pero las latencias de replicación pueden provocar incoherencias si se ejecuta el comando **dfsrmig** con la opción **/GetGlobalState** en un controlador de dominio que no sea el emulador de PDC. Para comprobar el estado de la migración local de un controlador de dominio que no sea el emulador de PDC, use la opción **/GetMigrationState** en su lugar. | | /GetMigrationState | Recupera el estado actual de la migración local para todos los controladores de dominio del dominio y determina si esos Estados locales coinciden con el estado de migración global actual.<p>Use esta opción para determinar si todos los controladores de dominio han alcanzado el estado de migración global. La salida del comando **dsfrmig** cuando se usa la opción **/GetMigrationState** indica si la migración al estado global actual está completa o no, y muestra el estado de migración local para los controladores de dominio que no hayan alcanzado el estado de migración global actual. El estado de migración local para los controladores de dominio puede incluir Estados de transición para los controladores de dominio que no han alcanzado el estado de migración global actual. | | /createGlobalObjects | Crea los objetos y la configuración globales en AD DS que usa Replicación DFS.<p>No es necesario usar esta opción durante un proceso de migración normal, porque el servicio de Replicación DFS crea automáticamente estos AD DS objetos y valores de configuración durante la migración desde el estado iniciar al estado preparado. Utilice esta opción para crear manualmente estos objetos y valores de configuración en las siguientes situaciones:<p>-  **Durante la migración, se promociona un nuevo controlador de dominio de solo lectura**. El servicio Replicación DFS crea automáticamente los objetos y la configuración de AD DS para Replicación DFS durante la migración desde el estado iniciar al estado preparado. Si se promueve un nuevo controlador de dominio de solo lectura en el dominio después de esta transición, pero antes de la migración al estado eliminado, los objetos que se corresponden con el controlador de dominio de solo lectura que se acaba de activar no se crean en AD DS que provocan errores en la replicación y la migración.<br />-En este caso, puede ejecutar el comando **dfsrmig** funcionaba la opción **/createGlobalObjects** para crear manualmente los objetos en todos los controladores de dominio de solo lectura que aún no los tienen. Ejecutar este comando no afecta a los controladores de dominio que ya tienen los objetos y la configuración del servicio Replicación DFS.<p>- **La configuración global del servicio replicación DFS falta o se eliminó**. Si faltan estas opciones de configuración en un controlador de dominio determinado, la migración del estado de inicio al estado preparado se detiene en el estado de la transición de preparación para el controlador de dominio. En este caso, puede usar el comando **dfsrmig** con la opción **/createGlobalObjects** para crear manualmente la configuración. **Nota:** Dado que la configuración global del AD DS para el servicio de Replicación DFS para un controlador de dominio de solo lectura se crea en el emulador de PDC, esta configuración debe replicarse en el controlador de dominio de solo lectura desde el emulador de PDC antes de que el servicio de Replicación DFS del controlador de dominio de solo lectura pueda usar esta configuración. Debido a las latencias de replicación de directorio activas, esta replicación puede tardar algún tiempo en realizarse. | | /deleteRoNtfrsMember [<read_only_domain_controller_name>] | Elimina la configuración global de AD DS para la replicación de FRS que corresponde al controlador de dominio de solo lectura especificado o elimina la configuración de AD DS global para la replicación de FRS para todos los controladores de dominio de solo lectura si no se especifica ningún valor para *read_only_domain_controller_name*.<p>No es necesario usar esta opción durante un proceso de migración normal, porque el servicio de Replicación DFS elimina automáticamente estos valores de AD DS durante la migración desde el estado redirigido al estado eliminado. Dado que los controladores de dominio de solo lectura no pueden eliminar estos valores de AD DS, el emulador de PDC realiza esta operación y los cambios finalmente se replican en los controladores de dominio de solo lectura después de las latencias aplicables para la replicación de Active Directory.<p>Esta opción se usa para eliminar manualmente la configuración de AD DS solo cuando se produce un error en la eliminación automática en un controlador de dominio de solo lectura y detiene el controlador de dominio de solo lectura durante la migración desde el estado redirigido al estado eliminado. | | /deleteRoDfsrMember [<read_only_domain_controller_name>] | Elimina la configuración global de AD DS para Replicación DFS que se corresponde con el controlador de dominio de solo lectura especificado, o elimina la configuración de AD DS global de Replicación DFS para todos los controladores de dominio de solo lectura si no se especifica ningún valor para *read_only_domain_controller_name*.<p>Use esta opción para eliminar manualmente la configuración de AD DS solo cuando se produce un error en la eliminación automática en un controlador de dominio de solo lectura y detiene el controlador de dominio de solo lectura durante un período de tiempo prolongado al revertir la migración del estado preparado al estado Inicio. | | /? | Muestra la ayuda en el símbolo del sistema. Equivalente a ejecutar **dfsrmig** sin ninguna opción. |

## <a name="remarks"></a>Observaciones
- dfsrmig. exe, la herramienta de migración para el servicio de Replicación DFS, se instala con el servicio Replicación DFS.
 para un nuevo servidor de Windows Server 2008, Dcpromo. exe instala e inicia el servicio Replicación DFS al promover el equipo a un controlador de dominio. Cuando se actualiza un servidor de Windows Server 2003 a Windows Server 2008, el proceso de actualización instala e inicia el servicio de Replicación DFS. No es necesario instalar el servicio de rol de Replicación DFS para que el servicio Replicación DFS esté instalado e iniciado.
- La herramienta **dfsrmig** solo se admite en los controladores de dominio que se ejecutan en el nivel funcional de dominio de windows Server 2008, ya que la migración de SYSVOL desde FRS a replicación DFS solo es posible en los controladores de dominio que operan en el nivel funcional de dominio de windows Server 2008.
- Puede ejecutar el comando **dfsrmig** en cualquier controlador de dominio, pero las operaciones que crean o manipulan objetos AD DS solo se permiten en controladores de dominio compatibles con lectura y escritura (no en controladores de dominio de solo lectura).
- Al ejecutar **dfsrmig** sin ninguna opción, se muestra la ayuda en el símbolo del sistema.

## <a name="examples"></a>Ejemplos
Para establecer el estado de migración global en preparado (**1**) e iniciar la migración a o revertir desde el estado preparado, escriba:
 ```
 dfsrmig /SetGlobalState 1
 ```
 Para establecer el estado de migración global en Inicio (**0**) e iniciar reversión en el estado Inicio, escriba:
 ```
 dfsrmig /SetGlobalState 0
 ```
 Para mostrar el estado de migración global, escriba:
 ```
 dfsrmig /GetGlobalState
 ```
 En este ejemplo se muestra el resultado típico del comando **dfsrmig/GetGlobalState** .
 ```
 Current DFSR global state: Prepared 
 Succeeded.
 ```
 Para mostrar la información sobre si los Estados de migración local en todos los controladores de dominio coinciden con el estado de migración global y los Estados de migración local para los controladores de dominio en los que el estado local no coincide con el estado global, escriba:
 ```
 dfsrmig /GetMigrationState
 ```
 En este ejemplo se muestra el resultado típico del comando **dfsrmig/GetMigrationState** cuando los Estados de migración local en todos los controladores de dominio coinciden con el estado de migración global.
 ```
 All Domain Controllers have migrated successfully to Global state ( Prepared ).
 Migration has reached a consistent state on all Domain Controllers.
 Succeeded.
 ```
 En este ejemplo se muestra el resultado típico del comando **dfsrmig/GetMigrationState** cuando los Estados de migración local en algunos controladores de dominio no coinciden con el estado de migración global.
 ```
  The following Domain Controllers are not in sync with Global state ( Prepared ):
  Domain Controller (Local Migration State)  DC type
  =========
  CONTOSO-DC2 ( start )  ReadOnly DC
  CONTOSO-DC3 ( Preparing )  Writable DC
  Migration has not yet reached a consistent state on all domain controllers
  State information might be stale due to AD latency.
 ```
Para crear los objetos globales y la configuración que usa Replicación DFS en AD DS en controladores de dominio en los que esa configuración no se creó automáticamente durante la migración o donde faltan esas opciones, escriba:
```
dfsrmig /createGlobalObjects
```
Para eliminar la configuración global de AD DS para la replicación de FRS para un controlador de dominio de solo lectura denominado contoso-DC2 si la configuración no se eliminó automáticamente por el proceso de migración, escriba:
```
dfsrmig /deleteRoNtfrsMember contoso-dc2
```
Para eliminar la configuración global de AD DS para la replicación de FRS para todos los controladores de dominio de solo lectura si la configuración no se eliminó automáticamente por el proceso de migración, escriba:
```
dfsrmig /deleteRoNtfrsMember
```
Para eliminar la configuración de AD DS global de Replicación DFS para un controlador de dominio de solo lectura denominado contoso-DC2 si la configuración no se eliminó automáticamente en el proceso de migración, escriba:
```
dfsrmig /deleteRoDfsrMember contoso-dc2
```
Para eliminar la configuración de AD DS global de Replicación DFS para todos los controladores de dominio de solo lectura si la configuración no se eliminó automáticamente por el proceso de migración, escriba:
```
dfsrmig /deleteRoDfsrMember
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](https://go.microsoft.com/fwlink/?LinkId=122056)

- [Serie de migración de SYSvol: parte 2 dfsrmig. exe: herramienta de migración de SYSvol](https://go.microsoft.com/fwlink/?LinkID=121757)

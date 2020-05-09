---
title: dfsrmig
description: Tema de referencia del comando dfsrmig, que migra la replicación de SYSvol de FRS a Replicación DFS, proporciona información sobre el progreso de la migración y modifica los objetos de AD DS para admitir la migración.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e1b6a464-6a93-4e66-9969-04f175226d8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a0329bd5829941595f9e1938f68eefb17036c132
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992720"
---
# <a name="dfsrmig"></a>dfsrmig

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

La herramienta de migración para el servicio Replicación DFS, dfsrmig. exe, se instala con el servicio Replicación DFS. Esta herramienta migra la replicación de SYSvol desde el servicio de replicación de archivos (FRS) a la replicación de Sistema de archivos distribuido (DFS). También proporciona información sobre el progreso de la migración y modifica los objetos Active Directory Domain Services (AD DS) para admitir la migración.

## <a name="syntax"></a>Sintaxis

```
dfsrmig [/setglobalstate <state> | /getglobalstate | /getmigrationstate | /createglobalobjects |
/deleterontfrsmember [<read_only_domain_controller_name>] | /deleterodfsrmember [<read_only_domain_controller_name>] | /?]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `/setglobalstate <state>` | Establece el estado de migración global del dominio en uno que corresponde al valor especificado por *Estado*. Solo puede establecer el estado de migración global en un estado estable. Los valores de *Estado* incluyen:<ul><li>**0** -iniciar estado</li><li>**1** : estado preparado</li><li>**2** -estado Redirigido</li><li>**3** -estado eliminado</li></ul> |
| /getglobalstate | Recupera el estado de migración global actual para el dominio de la copia local de la base de datos de AD DS, cuando se ejecuta en el emulador de PDC. Use esta opción para confirmar que estableció el estado de migración global correcto.<p>**Importante:** Este comando solo debe ejecutarse en el emulador de PDC. |
| /getmigrationstate | Recupera el estado actual de la migración local para todos los controladores de dominio del dominio y determina si esos Estados locales coinciden con el estado de migración global actual. Use esta opción para determinar si todos los controladores de dominio han alcanzado el estado de migración global. |
| /createglobalobjects | Crea los objetos y la configuración globales en AD DS que usa Replicación DFS utiliza. Las únicas situaciones en las que se debe usar esta opción para crear manualmente objetos y configuraciones son:<ul><li>**Durante la migración, se promociona un nuevo controlador de dominio de solo lectura**. Si se promueve un nuevo controlador de dominio de solo lectura en el dominio después de pasar al estado **preparado** , pero antes de la migración al estado **eliminado** , no se crean los objetos que corresponden al nuevo controlador de dominio, lo que provocará un error en la replicación y la migración.</li><li>**Falta la configuración global del servicio replicación DFS o se eliminó**. Si faltan estas opciones de configuración en un controlador de dominio, la migración del estado de **Inicio** al estado **preparado** se detendrá en el estado **preparando** transición. **Nota:** Dado que la configuración global del AD DS para el servicio de Replicación DFS para un controlador de dominio de solo lectura se crea en el emulador de PDC, esta configuración debe replicarse en el controlador de dominio de solo lectura desde el emulador de PDC antes de que el servicio de Replicación DFS del controlador de dominio de solo lectura pueda usar esta configuración. Debido a Active Directory latencias de replicación, esta replicación puede tardar algún tiempo en realizarse. |
| `/deleterontfrsmember [<read_only_domain_controller_name>]` | Elimina la configuración global de AD DS para la replicación de FRS que corresponde al controlador de dominio de solo lectura especificado o elimina la configuración de AD DS global para la replicación de FRS para todos los controladores de dominio de solo lectura si `<read_only_domain_controller_name>`no se especifica ningún valor para.<p>No es necesario usar esta opción durante un proceso de migración normal, porque el servicio de Replicación DFS elimina automáticamente estos valores de AD DS durante la migración desde el estado **Redirigido** al estado **eliminado** . Use esta opción para eliminar manualmente la configuración de AD DS solo cuando se produce un error en la eliminación automática en un controlador de dominio de solo lectura y detiene el controlador de dominio de solo lectura durante la migración desde el estado **Redirigido** al estado **eliminado** . |
| `/deleterodfsrmember [<read_only_domain_controller_name>]` | Elimina la configuración global de AD DS para Replicación DFS que se corresponde con el controlador de dominio de solo lectura especificado, o elimina la configuración global de AD DS para Replicación DFS para todos los controladores de dominio de solo lectura si no `<read_only_domain_controller_name>`se especifica ningún valor para.<p>Use esta opción para eliminar manualmente la configuración de AD DS solo cuando se produce un error en la eliminación automática en un controlador de dominio de solo lectura y detiene el controlador de dominio de solo lectura durante un período de tiempo prolongado al revertir la migración del estado preparado al estado Inicio. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Comentarios

- Use el `/setglobalstate <state>` comando para establecer el estado de migración global en AD DS en el emulador de PDC para iniciar y controlar el proceso de migración. Si el emulador de PDC no está disponible, se produce un error en este comando.

- La migración al estado **eliminado** es irreversible y no es posible revertir, por lo que debe usar un valor de **3** para el *Estado* solo cuando esté totalmente confirmado para usar replicación DFS para la replicación de SYSvol.

- Los Estados de migración global deben ser un estado de migración estable.

- La replicación de Active Directory replica el estado global en otros controladores de dominio del dominio, pero debido a las latencias de replicación, puede obtener incoherencias si ejecuta `dfsrmig /getglobalstate` en un controlador de dominio que no sea el emulador de PDC.

- La salida de `dsfrmig /getmigrationstate` indica si se ha completado la migración al estado global actual, y muestra el estado de migración local para los controladores de dominio que aún no han alcanzado el estado de migración global actual. El estado de migración local para los controladores de dominio también puede incluir Estados de transición para los controladores de dominio que no han alcanzado el estado de migración global actual.

- Los controladores de dominio de solo lectura no pueden eliminar la configuración de AD DS, el emulador de PDC realiza esta operación y, finalmente, los cambios se replican en los controladores de dominio de solo lectura después de las latencias aplicables para la replicación de Active Directory.

- El comando **dfsrmig** solo se admite en los controladores de dominio que se ejecutan en el nivel funcional de dominio de Windows Server, ya que la migración de SYSVOL desde FRS a replicación DFS solo es posible en los controladores de dominio que operan en ese nivel.

- Puede ejecutar el comando **dfsrmig** en cualquier controlador de dominio, pero las operaciones que crean o manipulan objetos AD DS solo se permiten en controladores de dominio compatibles con lectura y escritura (no en controladores de dominio de solo lectura).

## <a name="examples"></a>Ejemplos

Para establecer el estado de migración global en preparado (**1**) y para iniciar la migración o para revertir el estado preparado, escriba:

```
dfsrmig /setglobalstate 1
```

Para establecer el estado de migración global en Inicio (**0**) y para iniciar la reversión al estado Inicio, escriba:

```
dfsrmig /setglobalstate 0
```

Para mostrar el estado de migración global, escriba:

```
dfsrmig /getglobalstate
```

Salida del `dfsrmig /getglobalstate` comando:

```
Current DFSR global state: Prepared
Succeeded.
```

Para mostrar información sobre si los Estados de migración local en todos los controladores de dominio coinciden con el estado de migración global y si hay algún Estado de migración local en el que el estado local no coincida con el estado global, escriba:

```
dfsrmig /GetMigrationState
```

Salida del `dfsrmig /getmigrationstate` comando cuando los Estados de migración local en todos los controladores de dominio coinciden con el estado de migración global:

```
All Domain Controllers have migrated successfully to Global state (Prepared).
Migration has reached a consistent state on all Domain Controllers.
Succeeded.
```

Salida del `dfsrmig /getmigrationstate` comando cuando los Estados de migración local en algunos controladores de dominio no coinciden con el estado de migración global.

```
The following Domain Controllers are not in sync with Global state (Prepared):
Domain Controller (Local Migration State) DC type
=========
CONTOSO-DC2 (start) ReadOnly DC
CONTOSO-DC3 (Preparing) Writable DC
Migration has not yet reached a consistent state on all domain controllers
State information might be stale due to AD latency.
```

Para crear los objetos globales y la configuración que usa Replicación DFS en AD DS en controladores de dominio en los que esa configuración no se creó automáticamente durante la migración o donde faltan esas opciones, escriba:

```
dfsrmig /createglobalobjects
```

Para eliminar la configuración global de AD DS para la replicación de FRS para un controlador de dominio de solo lectura denominado contoso-DC2 si la configuración no se eliminó automáticamente por el proceso de migración, escriba:

```
dfsrmig /deleterontfrsmember contoso-dc2
```

Para eliminar la configuración global de AD DS para la replicación de FRS para todos los controladores de dominio de solo lectura si la configuración no se eliminó automáticamente por el proceso de migración, escriba:

```
dfsrmig /deleterontfrsmember
```

Para eliminar la configuración de AD DS global de Replicación DFS para un controlador de dominio de solo lectura denominado contoso-DC2 si la configuración no se eliminó automáticamente en el proceso de migración, escriba:

```
dfsrmig /deleterodfsrmember contoso-dc2
```

Para eliminar la configuración de AD DS global de Replicación DFS para todos los controladores de dominio de solo lectura si la configuración no se eliminó automáticamente por el proceso de migración, escriba:

```
dfsrmig /deleterodfsrmember
```

Para mostrar la ayuda en el símbolo del sistema:

```
dfsrmig
```

```
dfsrmig /?
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](https://go.microsoft.com/fwlink/?LinkId=122056)

- [Serie de migración de SYSvol: parte 2 dfsrmig. exe: herramienta de migración de SYSvol](https://techcommunity.microsoft.com/t5/storage-at-microsoft/sysvol-migration-series-part-2-8211-dfsrmig-exe-the-sysvol/ba-p/423470)

- [Active Directory Domain Services](../../identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview.md)

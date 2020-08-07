---
title: bitsadmin transfer
description: Artículo de referencia del comando bitsadmin Transfer, que transfiere uno o varios archivos.
ms.topic: article
ms.assetid: fe302141-b33a-4a05-835e-dc4fc4db7d5a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 06496dffa417bb611700a9d6cec2262fd4d6e690
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87880974"
---
# <a name="bitsadmin-transfer"></a>bitsadmin transfer

Transfiere uno o varios archivos. De forma predeterminada, el servicio BITSAdmin crea un trabajo de descarga que se ejecuta con prioridad **normal** y actualiza la ventana de comandos con información de progreso hasta que se completa la transferencia o hasta que se produce un error crítico.

El servicio completa el trabajo si transfiere correctamente todos los archivos y cancela el trabajo si se produce un error crítico. El servicio no crea el trabajo si no puede Agregar archivos al trabajo o si especifica un valor no válido para el *tipo* o *job_priority*. Para transferir más de un archivo, especifique varios `<RemoteFileName>-<LocalFileName>` pares. Los pares deben estar delimitados por espacios.

> [!NOTE]
> El comando BITSAdmin continúa ejecutándose si se produce un error transitorio. Para finalizar el comando, presione CTRL + C.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /transfer <name> [<type>] [/priority <job_priority>] [/ACLflags <flags>] [/DYNAMIC] <remotefilename> <localfilename>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| name | Nombre del trabajo. Este comando no puede ser un GUID. |
| type | Opcional. Establece el tipo de trabajo, incluido:<ul><li>**Descargar.** Valor predeterminado. Elija este tipo para los trabajos de descarga.</li><li>**Subir.** Elija este tipo para los trabajos de carga.</li></ul> |
| priority | Opcional. Establece la prioridad del trabajo, incluidos:<ul><li>FOREGROUND</li><li>HIGH</li><li>NORMAL</li><li>LOW</li></ul> |
| ACLflags | Opcional. Indica que desea mantener la información de propietario y ACL con el archivo que se está descargando. Especifique uno o varios de los valores, entre los que se incluyen:<ul><li>**o** copiar información del propietario con el archivo.</li><li>**g** -copiar información de grupo con el archivo.</li><li>**d** -copiar información de la lista de control de acceso discrecional (DACL) con el archivo.</li><li>**s** -copiar información de la lista de control de acceso de sistema (SACL) con el archivo.</li></ul> |
| /DYNAMIC | Configura el trabajo mediante [**BITS_JOB_PROPERTY_DYNAMIC_CONTENT**](/windows/win32/api/bits5_0/ne-bits5_0-bits_job_property_id), lo que relaja los requisitos del lado servidor. |
| remotefilename | Nombre del archivo después de transferirlo al servidor. |
| localfilename | Nombre del archivo que reside localmente. |

## <a name="examples"></a>Ejemplos

Para iniciar un trabajo de transferencia denominado *myDownloadJob*:

```
bitsadmin /transfer myDownloadJob http://prodserver/audio.wma c:\downloads\audio.wma
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)

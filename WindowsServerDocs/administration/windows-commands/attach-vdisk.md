---
title: attach vdisk
description: Tema de referencia del comando Attach vDisk, que conecta (a veces denominados montajes o superficies) un disco duro virtual (VHD) para que aparezca en el equipo host como una unidad de disco duro local.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 882ab875-0c14-4eb3-98ef-fd0e8fa40d9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91f988d1f84869874dbd0d6a25dce43ef5138066
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718908"
---
# <a name="attach-vdisk"></a>attach vdisk

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Adjunta (a veces denominados montajes o superficies) un disco duro virtual (VHD) para que aparezca en el equipo host como una unidad de disco duro local. Si el VHD ya tiene una partición de disco y un volumen de sistema de archivos cuando lo adjunta, al volumen dentro del VHD se le asigna una letra de unidad.

> [!IMPORTANT]
> Debe elegir y desasociar un VHD para que esta operación se realice correctamente. Use el comando **Select vDisk** para seleccionar un disco duro virtual y desplazar el foco a él.

## <a name="syntax"></a>Sintaxis

```
attach vdisk [readonly] { [sd=<SDDL>] | [usefilesd] } [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| readonly | Conecta el VHD como de solo lectura. Cualquier operación de escritura devuelve un error. |
| `sd=<SDDL string>` | Establece el filtro de usuario en el disco duro virtual. La cadena de filtro debe estar en el formato de lenguaje de definición de descriptores de seguridad (SDDL). De forma predeterminada, el filtro de usuario permite el acceso como en un disco físico. Las cadenas SDDL pueden ser complejas, pero en su forma más simple, un descriptor de seguridad que protege el acceso se conoce como una lista de control de acceso discrecional (DACL). Utiliza el formato: `D:<dacl_flags><string_ace1><string_ace2>`...`<string_acen>`<p>Las marcas DACL comunes son:<ul><li>**Un**. Permitir acceso</li><li>**D**. Denegación del acceso</li></ul>Los derechos comunes son:<ul><li>**GA**. Todo el acceso</li><li>**Gr**. acceso de lectura</li><li> **GW**. Acceso de escritura</li></ul>Las cuentas de usuario comunes son:<ul><li>**BA**. Administradores integrados</li><li>**Au**. Usuarios autenticados</li><li>**Co**. Propietario del creador</li><li>**WD**. Todos</li></ul>Ejemplos:<ul><li>**D:P: (A;; GR;;; AU**. Concede acceso de lectura a todos los usuarios autenticados.</li><li>**D:P: (A;; GA;;; WD**. Concede a todos los usuarios acceso completo.</li></ul> |
| usefilesd | Especifica que el descriptor de seguridad del archivo. VHD debe usarse en el disco duro virtual. Si no se especifica el parámetro **Usefilesd** , el disco duro virtual no tendrá un descriptor de seguridad explícito a menos que se especifique con el parámetro **SD** . |
| noerr | Se usa solo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="examples"></a>Ejemplos

Para asociar el VHD seleccionado como de solo lectura, escriba:

```
attach vdisk readonly
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [seleccionar vDisk](select-vdisk.md)

- [Compact vDisk](compact-vdisk.md)

- [detalles del vDisk](detail-vdisk.md)

- [detach vdisk](detach-vdisk.md)

- [expandir vDisk](expand-vdisk.md)

- [Merge vDisk](merge-vdisk.md)

- [list](list_1.md)

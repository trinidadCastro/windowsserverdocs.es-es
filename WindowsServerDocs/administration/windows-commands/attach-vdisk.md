---
title: attach vdisk
description: El tema comandos de Windows para **Attach vDisk**, que conecta (a veces denominados montajes o superficies) un disco duro virtual (VHD) para que aparezca en el equipo host como una unidad de disco duro local.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 882ab875-0c14-4eb3-98ef-fd0e8fa40d9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3a903ed231e34ac902ce10b5342f27e772ac89f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851268"
---
# <a name="attach-vdisk"></a>attach vdisk

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adjunta (a veces denominados montajes o superficies) un disco duro virtual (VHD) para que aparezca en el equipo host como una unidad de disco duro local. Si el VHD ya tiene una partición de disco y un volumen de sistema de archivos cuando lo adjunta, al volumen dentro del VHD se le asigna una letra de unidad.

> [!NOTE]
> Este comando solo es aplicable a Windows 7 y Windows Server 2008 R2.

## <a name="syntax"></a>Sintaxis

```
attach vdisk [readonly] { [sd=<SDDL>] | [usefilesd] } [noerr]
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| readonly | Conecta el VHD como de solo lectura. Cualquier operación de escritura devuelve un error. |
| `sd=<SDDL string>` | Establece el filtro de usuario en el disco duro virtual. La cadena de filtro debe estar en el formato de lenguaje de definición de descriptores de seguridad (SDDL). De forma predeterminada, el filtro de usuario permite el acceso como en un disco físico. Las cadenas SDDL pueden ser complejas, pero en su forma más simple, un descriptor de seguridad que protege el acceso se conoce como una lista de control de acceso discrecional (DACL). Tiene el siguiente formato: `D:<dacl_flags><string_ace1><string_ace2>`... `<string_acen>`<p>Las marcas DACL comunes son:<p>-  **.** Permitir acceso<p>- **D**. Denegar acceso<p>Los derechos comunes son:<p>- **GA**. Todo el acceso<p>- **gr**. Acceso de lectura<p>- **GW**. Acceso de escritura<p>Las cuentas de usuario comunes son:<p>- **BA**. Administradores integrados<p>- **au**. Usuarios autenticados<p>- **Co**. Propietario del creador<p>- **WD**. Todos<p>Ejemplos:<p>**D:P: (A;; GR;;; AU** proporciona acceso de lectura a todos los usuarios autenticados.<p>**D:P: (A;; GA;;; WD** concede acceso completo a todos los usuarios. |
| usefilesd | Especifica que el descriptor de seguridad del archivo. VHD debe usarse en el disco duro virtual. Si no se especifica el parámetro **Usefilesd** , el disco duro virtual no tendrá un descriptor de seguridad explícito a menos que se especifique con el parámetro **SD** . |
| noerr | Se usa solo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="remarks"></a>Comentarios

- Para que esta operación se realice correctamente, se debe seleccionar y desasociar un disco duro virtual. Use el comando **Select vDisk** para seleccionar un disco duro virtual y desplazar el foco a él.

## <a name="examples"></a><a name=BKMK_Examples></a>Example

Para asociar el VHD seleccionado como de solo lectura, escriba:

```
attach vdisk readonly
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Compact vDisk](compact-vdisk.md)

- [detalles del vDisk](detail-vdisk.md)

- [detach vDisk](detach-vdisk.md)

- [expandir vDisk](expand-vdisk.md)

- [Merge vDisk](merge-vdisk.md)

- [seleccionar vDisk](select-vdisk.md)

- [lista](list_1.md)

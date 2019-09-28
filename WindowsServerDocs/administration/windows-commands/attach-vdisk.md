---
title: attach vdisk
description: 'El tema comandos de Windows para **Attach vDisk** : conecta (a veces denominados montajes o superficies) un disco duro virtual (VHD) para que aparezca en el equipo host como una unidad de disco duro local.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 882ab875-0c14-4eb3-98ef-fd0e8fa40d9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d29eacfc8575ec50859733612a3d58b166d9402d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382641"
---
# <a name="attach-vdisk"></a>attach vdisk

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

adjunta (a veces denominados montajes o superficies) un disco duro virtual (VHD) para que aparezca en el equipo host como una unidad de disco duro local. Si el VHD ya tiene una partición de disco y un volumen de sistema de archivos cuando lo adjunta, al volumen dentro del VHD se le asigna una letra de unidad.
> [!NOTE]
> Este comando solo es aplicable a Windows 7 y Windows Server 2008 R2.

## <a name="syntax"></a>Sintaxis
```
attach vdisk [readonly] { [sd=<SDDL>] | [usefilesd] } [noerr]
```
### <a name="parameters"></a>Parámetros

|    Parámetro     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          Descripción                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     ReadOnly     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                             conecta el VHD como de solo lectura. Cualquier operación de escritura devuelve un error.                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| SD = <SDDL string> | Establece el filtro de usuario en el disco duro virtual. La cadena de filtro debe estar en el formato de lenguaje de definición de descriptores de seguridad (SDDL). De forma predeterminada, el filtro de usuario permite el acceso como en un disco físico.<br /><br />Las cadenas SDDL pueden ser complejas, pero en su forma más simple, un descriptor de seguridad que protege el acceso se conoce como una lista de control de acceso discrecional (DACL). Tiene el siguiente formato: D: < dacl_flags > < string_ace1 > < string_ace2 >... < string_acen ><br /><br />Las marcas DACL comunes son:<br /><br />@no__t **-0 a** permitir acceso<br />-   **D** denegar el acceso<br /><br />Los derechos comunes son:<br /><br />-   **GA** All Access<br />-   **gr** acceso de lectura<br />acceso de escritura de -    de**GW**<br /><br />Las cuentas de usuario comunes son:<br /><br />-   **BA** integrado en los administradores<br />usuarios autenticados de -   **au**<br />propietario de**Co** Creator -   <br />-   **WD** -todos<br /><br />Ejemplos:<br /><br />**D:P: (A;; GR;;; AU** proporciona acceso de lectura a todos los usuarios autenticados<br /><br />**D:P: (A;; GA;;; WD** permite que todo el mundo tenga pleno alcance |
|    usefilesd     |                                                                                                                                                                                                                                                                                                                                                                                          Especifica que el descriptor de seguridad del archivo. VHD debe usarse en el disco duro virtual. Si no se especifica el parámetro **Usefilesd** , el disco duro virtual no tendrá un descriptor de seguridad explícito a menos que se especifique con el parámetro **SD** .                                                                                                                                                                                                                                                                                                                                                                                          |
|      Noerr       |                                                                                                                                                                                                                                                                                                                                                                                                           Se usa solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error.                                                                                                                                                                                                                                                                                                                                                                                                           |

## <a name="remarks"></a>Comentarios
- Para que esta operación se realice correctamente, se debe seleccionar y desasociar un disco duro virtual. Use el comando **Select vDisk** para seleccionar un disco duro virtual y desplazar el foco a él.
  ## <a name="BKMK_Examples"></a>Example
  Para asociar el VHD seleccionado como de solo lectura, escriba:
  ```
  attach vdisk readonly
  ```
  ## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [Compact vDisk](compact-vdisk.md)

- [detalles del vDisk](detail-vdisk.md)
- [Detach vDisk](detach-vdisk.md)
- [expandir vDisk](expand-vdisk.md)
- [Merge vDisk](merge-vdisk.md)
- [seleccionar vDisk](select-vdisk.md)
- [list_1](list_1.md)

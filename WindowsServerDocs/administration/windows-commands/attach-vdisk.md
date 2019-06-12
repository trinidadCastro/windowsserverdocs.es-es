---
title: attach vdisk
description: Tema de los comandos de Windows para **attach vdisk** -adjunta (denominadas en ocasiones montajes o superficies) un disco duro virtual (VHD) para que aparezca en el equipo host como una unidad de disco duro local.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: fb33d040ce0b2a7a9d06951a7e80251a0d0da614
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435224"
---
# <a name="attach-vdisk"></a>attach vdisk

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

adjunta (denominadas en ocasiones montajes o superficies) un disco duro virtual (VHD) para que aparezca en el equipo host como una unidad de disco duro local. Si el VHD ya tiene una partición de disco y un volumen de sistema de archivos cuando lo adjunta, al volumen dentro del VHD se le asigna una letra de unidad.
> [!NOTE]
> Este comando sólo es aplicable a Windows 7 y Windows Server 2008 R2.

## <a name="syntax"></a>Sintaxis
```
attach vdisk [readonly] { [sd=<SDDL>] | [usefilesd] } [noerr]
```
### <a name="parameters"></a>Parámetros

|    Parámetro     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          Descripción                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     ReadOnly     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                             adjunta el disco duro virtual como de solo lectura. Cualquier escritura operación devuelve un error.                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| sd=<SDDL string> | Establece el filtro de usuarios en el disco duro virtual. La cadena de filtro debe tener el formato de lenguaje de definición de descriptores de seguridad (SDDL). De forma predeterminada el filtro de usuarios permite el acceso al igual que en un disco físico.<br /><br />Las cadenas SDDL pueden ser complejas, pero en su forma más simple, un descriptor de seguridad que protege el acceso se conoce como una lista de control de acceso discrecional (DACL). Es el formato: D:<dacl_flags><string_ace1><string_ace2>... <string_acen><br /><br />Los indicadores DACL comunes son:<br /><br />-   **Un** permitir el acceso<br />-   **D.** denegar el acceso<br /><br />Derechos comunes son:<br /><br />-   **GA** todo el acceso<br />-   **GR** acceso de lectura<br />-   **GW** acceso de escritura<br /><br />Cuentas de usuario comunes son:<br /><br />-   **BA** administradores integrados<br />-   **AU** usuarios autenticados<br />-   **CO** Creator owner<br />-   **WD** -todo el mundo<br /><br />Ejemplos:<br /><br />**D:P:(A;; GR;; AU** proporciona acceso de lectura a todos los usuarios autenticados<br /><br />**D:P:(A;; DISPONIBILIDAD GENERAL;; WD** a todos los integrantes total acceso a |
|    usefilesd     |                                                                                                                                                                                                                                                                                                                                                                                          Especifica que se debe usar el descriptor de seguridad en el archivo .vhd en el disco duro virtual. Si el **Usefilesd** parámetro no se especifica, el disco duro virtual no tendrá un descriptor de seguridad explícitos a menos que se especifica con el **Sd** parámetro.                                                                                                                                                                                                                                                                                                                                                                                          |
|      noerr       |                                                                                                                                                                                                                                                                                                                                                                                                           Se utiliza sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.                                                                                                                                                                                                                                                                                                                                                                                                           |

## <a name="remarks"></a>Comentarios
- Un disco duro virtual debe estar seleccionado y desconectado para que esta operación se realice correctamente. Use la **seleccione vdisk** comando para seleccionar un disco duro virtual y desplace el foco a ella.
  ## <a name="BKMK_Examples"></a>Ejemplos
  Para adjuntar el disco duro virtual seleccionado como de solo lectura, escriba:
  ```
  attach vdisk readonly
  ```
  ## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [compact vdisk](compact-vdisk.md)

- [vdisk detallado](detail-vdisk.md)
- [Desasociar vdisk](detach-vdisk.md)
- [Expandir vdisk](expand-vdisk.md)
- [Combinar vdisk](merge-vdisk.md)
- [select vdisk](select-vdisk.md)
- [list_1](list_1.md)

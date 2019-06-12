---
title: freedisk
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 91c15166-5baa-4b80-9e0c-4cd815d00530
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ac2f38fb1948a26ae55713ac6fb14254851b062a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439159"
---
# <a name="freedisk"></a>freedisk

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comprueba si la cantidad de espacio en disco especificada está disponible antes de continuar con un proceso de instalación.

## <a name="syntax"></a>Sintaxis
```
freedisk [/s <computer> [/u [<Domain>\]<User> [/p [<Password>]]]] [/d <Drive>] [<Value>]
```
## <a name="parameters"></a>Parámetros

|       Parámetro       |                                                                                         Descripción                                                                                          |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /s <computer>     | Especifica el nombre o dirección IP de un equipo remoto (no utilice las barras diagonales inversas). El valor predeterminado es el equipo local. Este parámetro se aplica a todos los archivos y carpetas especificadas en el comando. |
| /u [<Domain>\\]<User> |                                            Ejecuta la secuencia de comandos con los permisos de la cuenta de usuario especificado. El valor predeterminado es permisos del sistema.                                            |
|    /p [<Password>]    |                                                           Especifica la contraseña de la cuenta de usuario que se especifica en **/u**.                                                            |
|      /d <Drive>       |                              Especifica la unidad para el que desea averiguar la disponibilidad de espacio libre. Debe especificar <Drive>para un equipo remoto.                               |
|        <Value>        |                                     Comprueba si hay una cantidad específica de espacio libre en disco. Puede especificar <Value>en bytes, KB, MB, GB, TB, PB, EB, ZB o YB.                                      |

## <a name="remarks"></a>Comentarios
- Mediante el **/s**, **/u**, y **/p** opciones de línea de comandos están disponibles solo cuando se usa **/s**. Debe usar **/p** con **/u**para proporcionar la contraseña del usuario s.
- para las instalaciones desatendidas, puede usar **freedisk** en archivos por lotes de instalación para comprobar el espacio libre de la cantidad de requisitos previos antes de continuar con la instalación.
- Cuando usas **freedisk** en un archivo por lotes, devuelve un **0** si hay suficiente espacio y un **1** si no hay suficiente espacio.
  ## <a name="BKMK_examples"></a>Ejemplos
  Para determinar si hay al menos 50 MB de espacio libre disponible en la unidad C:, escriba:
  ```
  freedisk 50mb 
  ```
  En la pantalla aparecerá un resultado similar al siguiente:
  ```
  INFO: The specified 52,428,800 byte(s) of free space is available on current drive.
  ```
  ## <a name="additional-references"></a>Referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

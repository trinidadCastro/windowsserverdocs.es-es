---
title: bootcfg timeout
description: Tema de los comandos de Windows para **tiempo de espera de bootcfg** -cambia el valor de tiempo de espera del sistema operativo.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aa858eac-2bb7-4a27-a9bc-3e4a6eb8b2c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1fc33d2d20d6d2532c5ed1f33e27a768935d1e85
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434642"
---
# <a name="bootcfg-timeout"></a>bootcfg timeout

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia el valor de tiempo de espera del sistema operativo.

## <a name="syntax"></a>Sintaxis
```
bootcfg /timeout <timeOutValue> [/s <computer> [/u <Domain\User>/p <Password>]]
```
## <a name="parameters"></a>Parámetros

|        Parámetro        |                                                                                                                                                                                  Descripción                                                                                                                                                                                   |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| / Timeout <timeOutValue> | Especifica el valor de tiempo de espera en la sección [boot loader]. El <timeOutValue> es el número de segundos que el usuario tiene que seleccionar un sistema operativo de la pantalla del cargador de arranque antes de que NTLDR cargue el valor predeterminado. Intervalo válido para <timeOutValue> es 0 y 999. Si el valor es 0, NTLDR iniciará inmediatamente el sistema operativo predeterminado sin mostrar la pantalla del cargador de arranque. |
|      /s <computer>      |                                                                                                                               Especifica el nombre o dirección IP de un equipo remoto (no utilice las barras diagonales inversas). El valor predeterminado es el equipo local.                                                                                                                               |
|    /u <Domain\User>     |                                                                                       Ejecuta el comando con los permisos de cuenta del usuario especificado por <User> o < DOMINIO\nombre de usuario >. El valor predeterminado es los permisos de la sesión de usuario en el equipo que emite el comando actual.                                                                                        |
|      /p <Password>      |                                                                                                                                            Especifica el <Password> de la cuenta de usuario que se especifica en el **/u** parámetro.                                                                                                                                             |
|           /?            |                                                                                                                                                                      Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                      |

## <a name="BKMK_examples"></a>Ejemplos
Los ejemplos siguientes muestran cómo puede usar el **bootcfg /timeout** comando:
```
bootcfg /timeout 30
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /timeout 50
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

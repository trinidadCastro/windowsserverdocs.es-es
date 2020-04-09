---
title: bootcfg timeout
description: Tema de comandos de Windows para Bootcfg timeout, que cambia el valor de tiempo de espera del sistema operativo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aa858eac-2bb7-4a27-a9bc-3e4a6eb8b2c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b56e609d5e3b7c92a887a98ae5d02bfbfb7a78e6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848498"
---
# <a name="bootcfg-timeout"></a>bootcfg timeout

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cambia el valor de tiempo de espera del sistema operativo.

## <a name="syntax"></a>Sintaxis

```
bootcfg /timeout <timeOutValue> [/s <computer> [/u <Domain\User>/p <Password>]]
```

### <a name="parameters"></a>Parámetros


|        Parámetro        |                                                                                                                                                                                  Descripción                                                                                                                                                                                   |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /timeout <timeOutValue> | Especifica el valor de tiempo de espera en la sección [boot loader]. El <timeOutValue> es el número de segundos que el usuario tiene para seleccionar un sistema operativo en la pantalla del cargador de arranque antes de que NTLDR cargue el valor predeterminado. El intervalo válido para <timeOutValue> es 0-999. Si el valor es 0, NTLDR inicia inmediatamente el sistema operativo predeterminado sin mostrar la pantalla del cargador de arranque. |
|      /s <computer>      |                                                                                                                               Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local.                                                                                                                               |
|    /u < Dominio\usuario >     |                                                                                       Ejecuta el comando con los permisos de cuenta del usuario especificado mediante <User> o < Dominio\usuario >. El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando.                                                                                        |
|      /p <Password>      |                                                                                                                                            Especifica el <Password> de la cuenta de usuario que se especifica en el parámetro **/u** .                                                                                                                                             |
|           /?            |                                                                                                                                                                      Muestra la Ayuda en el símbolo del sistema.                                                                                                                                                                      |

## <a name="examples"></a><a name=BKMK_examples></a>Example
En los siguientes ejemplos se muestra cómo se puede usar el comando **bootcfg/timeout** :
```
bootcfg /timeout 30
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /timeout 50
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

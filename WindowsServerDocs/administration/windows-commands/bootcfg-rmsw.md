---
title: bootcfg rmsw
description: Tema de referencia para el comando bootcfg Rmsw, que quita las opciones de carga del sistema operativo para una entrada específica del sistema operativo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fd7e4248-880e-4e2b-929e-87f8d44b9a63
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 41c9819fb3d669b24a5918077bef960869625a15
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82708912"
---
# <a name="bootcfg-rmsw"></a>bootcfg rmsw

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Quita las opciones de carga del sistema operativo para una entrada de sistema operativo especificada.

## <a name="syntax"></a>Sintaxis

```
bootcfg /rmsw [/s <computer> [/u <domain>\<user> /p <password>]] [/mm] [/bv] [/so] [/ng] /id <osentrylinenum>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `/s <computer>` | Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local. |
| `/u <domain>\<user>`  | Ejecuta el comando con los permisos de cuenta del usuario especificado por `<user>` o `<domain>\<user>`. El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando. |
| `/p <password>` | Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** . |
| Val | Quita la opción/MaxMem y su valor máximo de memoria asociado del especificado `<osentrylinenum>`. La opción/MaxMem especifica la cantidad máxima de memoria RAM que puede usar el sistema operativo. |
| /bv | Quita la opción/basevideo del especificado `<osentrylinenum>`. La opción/basevideo indica al sistema operativo que use el modo VGA estándar para el controlador de vídeo instalado. |
| /so | Quita la opción/SOS del especificado `<osentrylinenum>`. La opción/SOS indica al sistema operativo que muestre los nombres de los controladores de dispositivo mientras se cargan. |
| /NG | Quita la opción/noguiboot del especificado `<osentrylinenum>`. La opción/noguiboot deshabilita la barra de progreso que aparece antes del símbolo del sistema de inicio de sesión CTRL + ALT + SUPR. |
| `/id <osentrylinenum>` | Especifica el número de línea de entrada del sistema operativo en la sección [operating systems] del archivo boot. ini en el que se agregan las opciones de carga del sistema operativo. La primera línea después del encabezado de la sección [operating systems] es 1. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para usar el comando **bootcfg/Rmsw** :

```
bootcfg /rmsw /mm 64 /id 2
bootcfg /rmsw /so /id 3
bootcfg /rmsw /so /ng /s srvmain /u hiropln /id 2
bootcfg /rmsw /ng /id 2
bootcfg /rmsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando bootcfg](bootcfg.md)

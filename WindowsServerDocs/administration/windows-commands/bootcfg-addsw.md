---
title: bootcfg addsw
description: Artículo de referencia para el comando bootcfg addsw, que agrega opciones de carga del sistema operativo para una entrada específica del sistema operativo.
ms.topic: reference
ms.assetid: d8389293-ecd9-42f0-b84b-b9ead4cf52e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e47d9001dc9b2bac7f08036f15408a1e35aaf075
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89034553"
---
# <a name="bootcfg-addsw"></a>bootcfg addsw

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Agrega opciones de carga del sistema operativo para una entrada específica del sistema operativo.

## <a name="syntax"></a>Sintaxis

```
bootcfg /addsw [/s <computer> [/u <domain>\<user> /p <password>]] [/mm <maximumram>] [/bv] [/so] [/ng] /id <osentrylinenum>
```

### <a name="parameters"></a>Parámetros

| Término | Definición |
| ---- | ---------- |
| `/s <computer>` | Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). La opción predeterminada es el equipo local. |
| `/u <domain>\<user>`  | Ejecuta el comando con los permisos de cuenta del usuario especificado por `<user>` o `<domain>\<user>` . El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando. |
| `/p <password>` | Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** . |
| `/mm <maximumram>` | Especifica la cantidad máxima de RAM, en megabytes, que puede usar el sistema operativo. El valor debe ser igual o mayor que 32 megabytes. |
| /bv | Agrega la opción **/basevideo** al especificado `<osentrylinenum>` y dirige al sistema operativo para que use el modo VGA estándar para el controlador de vídeo instalado. |
| /so | Agrega la opción **/SOS** al especificado `<osentrylinenum>` y dirige al sistema operativo para que muestre los nombres de los controladores de dispositivo mientras se cargan. |
| /NG | Agrega la opción **/noguiboot** al especificado `<osentrylinenum>` , deshabilitando la barra de progreso que aparece antes del símbolo del sistema de inicio de sesión Ctrl + Alt + Supr. |
| `/id <osentrylinenum>` | Especifica el número de línea de entrada del sistema operativo en la sección [sistemas operativos] del archivo de Boot.ini al que se agregan las opciones de carga del sistema operativo. La primera línea después del encabezado de la sección [operating systems] es 1. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para usar el comando **bootcfg/addsw** :

```
bootcfg /addsw /mm 64 /id 2
bootcfg /addsw /so /id 3
bootcfg /addsw /so /ng /s srvmain /u hiropln /id 2
bootcfg /addsw /ng /id 2
bootcfg /addsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando bootcfg](bootcfg.md)

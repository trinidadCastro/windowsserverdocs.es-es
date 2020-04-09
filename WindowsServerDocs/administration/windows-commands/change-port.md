---
title: change port
description: Comandos de Windows tema para cambiar el puerto, que enumera o cambia las asignaciones de puertos COM para que sean compatibles con las aplicaciones de MS-DOS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3d772c90-e849-4e74-b9ec-b6cae1159336 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 39273382038edb7709f2d99baea8090d71df3a57
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848078"
---
# <a name="change-port"></a>change port

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Enumera o cambia las asignaciones de puertos COM para que sean compatibles con las aplicaciones de MS-DOS.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services ha cambiado a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis

```
change port [<PortX>=<PortY| /d <PortX| /query]
```

### <a name="parameters"></a>Parámetros


|    Parámetro    |              Descripción               |
|-----------------|----------------------------------------|
| <PortX>=<PortY> | Asigna*COM < no*está > para <*Puerto*> |
|   /d <PortX>    | Elimina la asignación para*COM < no* está> |
|     /Query      | Muestra las asignaciones de puerto actuales |
|       /?        | Muestra la ayuda en el símbolo del sistema |

## <a name="remarks"></a>Comentarios

- La mayoría de las aplicaciones de MS-DOS solo admiten puertos serie de COM1 a COM4. El comando **cambiar puerto** asigna un puerto serie a un número de puerto diferente, lo que permite que las aplicaciones que no admiten puertos com de número alto tengan acceso al puerto serie. la reasignación solo funciona para la sesión actual y no se conserva si cierra sesión en una sesión y vuelve a iniciarla.

- Use **cambiar puerto** sin parámetros para mostrar los puertos com disponibles y sus asignaciones actuales.

## <a name="examples"></a><a name=BKMK_examples></a>Example

- Para asignar COM12 a COM1 para su uso por parte de una aplicación basada en MS-dos, escriba:
  ```
  change port com12=com1
  ```
- Para mostrar las asignaciones de puertos actuales, escriba:
  ```
  change port /query
  ```

### <a name="additional-references"></a>Referencias adicionales
- - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [change](change.md)
- [Referencia de comandos (Terminal Services) de Servicios de Escritorio remoto](remote-desktop-services-terminal-services-command-reference.md)

---
description: 'Más información acerca de: Servicio de mantenimiento de acciones'
title: Servicio de mantenimiento acciones
manager: eldenc
ms.author: cosdar
ms.topic: article
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: 28091a02c590325bb53e36b132aaa5fc667437dd
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97042243"
---
# <a name="health-service-actions"></a>Servicio de mantenimiento acciones

> Se aplica a: Windows Server 2019, Windows Server 2016

El Servicio de mantenimiento es una nueva característica de Windows Server 2016 que mejora la supervisión cotidiana y la experiencia operativa de los clústeres que ejecutan Espacios de almacenamiento directo.

## <a name="actions"></a>Acciones

En la sección siguiente se describen los flujos de trabajo que se automatizan mediante el servicio de mantenimiento. Para comprobar que se realiza una acción de forma autónoma, o para realizar el seguimiento de su progreso o resultado, el servicio de mantenimiento genera "acciones". A diferencia de los registros, las acciones desaparecen poco después de haberse completado y están pensadas principalmente para proporcionar información sobre la actividad en curso que pueda afectar al rendimiento o a la capacidad (por ejemplo, restauración de la resistencia o reequilibrio de datos).

### <a name="usage"></a>Uso

Un nuevo cmdlet de PowerShell muestra todas las acciones:

```PowerShell
Get-StorageHealthAction
```

### <a name="coverage"></a>Cobertura

En Windows Server 2016, el cmdlet **Get-StorageHealthAction** puede devolver cualquiera de los siguientes datos:

-   Error en la retirada, conectividad perdida o disco físico no responde

-   Cambio del grupo de almacenamiento para utilizar el disco físico de reemplazo

-   Restauración completa de la resistencia a los datos

-   Reequilibrio del grupo de almacenamiento

## <a name="additional-references"></a>Referencias adicionales

- [Servicio de mantenimiento de Windows Server 2016](health-service-overview.md)
- [Documentación para desarrolladores, código de ejemplo y referencia de API en MSDN](https://msdn.microsoft.com/windowshealthservice)

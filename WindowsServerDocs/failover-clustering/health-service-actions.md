---
title: Acciones de servicio de mantenimiento
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: efdf8f04e68fcbdc7051e78d6725cb919e740ffa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843026"
---
# <a name="health-service-actions"></a>Acciones de servicio de mantenimiento

> Se aplica a Windows Server 2016

El servicio de mantenimiento es una característica nueva en Windows Server 2016 que mejora la supervisión diaria y la experiencia operativa para los clústeres que ejecutan espacios de almacenamiento directo.

## <a name="actions"></a>Acciones  

En la sección siguiente se describen los flujos de trabajo que se automatizan mediante el servicio de mantenimiento. Para comprobar que se realiza una acción de forma autónoma, o para realizar el seguimiento de su progreso o resultado, el servicio de mantenimiento genera "acciones". A diferencia de los registros, las acciones desaparecen poco después de haberse completado y están pensadas principalmente para proporcionar información sobre la actividad en curso que pueda afectar al rendimiento o a la capacidad (por ejemplo, restauración de la resistencia o reequilibrio de datos).  

### <a name="usage"></a>Uso  

Un nuevo cmdlet de PowerShell muestra todas las acciones:  

```PowerShell
Get-StorageHealthAction  
```

### <a name="coverage"></a>Cobertura  

En Windows Server 2016, el **Get-StorageHealthAction** cmdlet puede devolver cualquiera de la siguiente información:  

-   Error en la retirada, conectividad perdida o disco físico no responde  

-   Cambio del grupo de almacenamiento para utilizar el disco físico de reemplazo  

-   Restauración completa de la resistencia a los datos  

-   Reequilibrio del grupo de almacenamiento  

## <a name="see-also"></a>Vea también

- [Servicio de mantenimiento de Windows Server 2016](health-service-overview.md)
- [Referencia de API en MSDN, código de ejemplo y documentación para desarrolladores](https://msdn.microsoft.com/windowshealthservice)

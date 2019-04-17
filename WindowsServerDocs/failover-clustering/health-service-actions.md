---
title: Acciones de servicio de estado
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: efdf8f04e68fcbdc7051e78d6725cb919e740ffa
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="health-service-actions"></a>Acciones de servicio de estado

> Se aplica a Windows Server 2016

El servicio de estado es una nueva característica de Windows Server 2016 que mejora la supervisión diarias y de la experiencia operativa clústeres ejecutan directa de los espacios de almacenamiento.

## <a name="actions"></a>Acciones  

La siguiente sección describe los flujos de trabajo que estén automatizadas por el servicio de estado. Para comprobar que se realice una acción efectivamente se forma autónoma, o para realizar un seguimiento de su progreso o el resultado, el servicio de estado genera "Acciones". A diferencia de los registros, acciones desaparecen poco después de que han completado y están diseñados principalmente para proporcionar información sobre la actividad en curso, lo que puede afectar al rendimiento o la capacidad (por ejemplo, restaurar el estado de resistencia o volver a equilibrar datos).  

### <a name="usage"></a>Uso de  

Una nuevo cmdlet de PowerShell muestra todas las acciones:  

```PowerShell
Get-StorageHealthAction  
```

### <a name="coverage"></a>Cobertura  

En Windows Server 2016, la **Get StorageHealthAction** cmdlet puede devolver cualquiera de la siguiente información:  

-   Retirada de conectividad, error de pérdida o disco físico no responde  

-   Cambiar el grupo de almacenamiento para utilizar disco físico de reemplazo  

-   Restaurar el estado de resistencia completo a datos  

-   Volver a equilibrar el grupo de almacenamiento  

## <a name="see-also"></a>Consulta también

- [Servicio de estado en Windows Server 2016](health-service-overview.md)
- [Documentación del desarrollador, el código de muestra y referencia de API en MSDN](https://msdn.microsoft.com/windowshealthservice)

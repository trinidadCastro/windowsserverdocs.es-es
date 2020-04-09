---
title: Servicio de mantenimiento acciones
ms.prod: windows-server
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: a4ef08a4ca552211b64d11677153775d6b18b4fa
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80827628"
---
# <a name="health-service-actions"></a>Servicio de mantenimiento acciones

> Se aplica a: Windows Server 2019, Windows Server 2016

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

## <a name="see-also"></a>Vea también

- [Servicio de mantenimiento en Windows Server 2016](health-service-overview.md)
- [Documentación para desarrolladores, código de ejemplo y referencia de API en MSDN](https://msdn.microsoft.com/windowshealthservice)

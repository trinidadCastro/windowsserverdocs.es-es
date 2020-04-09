---
title: Resistencia anidada para Espacios de almacenamiento directo
ms.prod: windows-server
ms.author: jgerend
manager: dansimp
ms.technology: storagespaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/15/2019
ms.openlocfilehash: ac4edccf0c1f8882dd2544b2544c3d8555bbc716
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857348"
---
# <a name="nested-resiliency-for-storage-spaces-direct"></a>Resistencia anidada para Espacios de almacenamiento directo

> Se aplica a: Windows Server 2019

La resistencia anidada es una nueva capacidad de [espacios de almacenamiento directo](storage-spaces-direct-overview.md) en Windows Server 2019 que permite que un clúster de dos servidores resista varios errores de hardware al mismo tiempo sin pérdida de disponibilidad de almacenamiento, por lo que los usuarios, las aplicaciones y las máquinas virtuales continúan ejecutándose sin interrupciones. En este tema se explica cómo funciona, se proporcionan instrucciones paso a paso para comenzar y se responde a las preguntas más frecuentes.

## <a name="prerequisites"></a>Requisitos previos

### <a name="green-checkmark-icon-consider-nested-resiliency-if"></a>![Icono de marca de verificación verde.](media/nested-resiliency/supported.png) Considere la resistencia anidada si:

- El clúster ejecuta Windows Server 2019; etc
- El clúster tiene exactamente 2 nodos de servidor

### <a name="red-x-icon-you-cant-use-nested-resiliency-if"></a>![Icono X de color rojo.](media/nested-resiliency/unsupported.png) No se puede usar la resistencia anidada si:

- El clúster ejecuta Windows Server 2016; de
- El clúster tiene 3 o más nodos de servidor

## <a name="why-nested-resiliency"></a>Motivos de resistencia anidada

Los volúmenes que usan resistencia anidada pueden **permanecer en línea y ser accesibles incluso si se producen varios errores de hardware al mismo tiempo**, a diferencia de la resistencia [de creación de reflejo bidireccional](storage-spaces-fault-tolerance.md) clásica. Por ejemplo, si se produce un error en dos unidades al mismo tiempo, o si un servidor deja de funcionar y se produce un error en una unidad, los volúmenes que usan la resistencia anidada permanecen en línea y accesibles. En el caso de la infraestructura hiperconvergida, esto aumenta el tiempo de actividad de las aplicaciones y las máquinas virtuales. en las cargas de trabajo de servidor de archivos, esto significa que los usuarios disfrutan de acceso ininterrumpido a sus archivos.

![Disponibilidad de almacenamiento](media/nested-resiliency/storage-availability.png)

La desventaja es que la resistencia anidada tiene **menos eficiencia de la capacidad que la creación de reflejo bidireccional clásica**, lo que significa que se obtiene un espacio ligeramente menos utilizable. Para obtener más información, consulte la sección eficiencia de la [capacidad](#capacity-efficiency) más adelante.

## <a name="how-it-works"></a>Cómo funciona

### <a name="inspiration-raid-51"></a>Inspiración: RAID 5 + 1

RAID 5 + 1 es una forma establecida de resistencia de almacenamiento distribuido que proporciona información útil para comprender la resistencia anidada. En RAID 5 + 1, en cada servidor, RAID-5, o *paridad única*, proporciona resistencia local para protegerse frente a la pérdida de una sola unidad. Después, RAID-1, o la *creación de reflejo bidireccional*, proporciona resistencia adicional entre los dos servidores para protegerse frente a la pérdida de cualquiera de los servidores.

![RAID 5 + 1](media/nested-resiliency/raid-51.png)

### <a name="two-new-resiliency-options"></a>Dos nuevas opciones de resistencia

Espacios de almacenamiento directo en Windows Server 2019 ofrece dos nuevas opciones de resistencia implementadas en el software sin necesidad de hardware de RAID especializado:

- **Reflejo doble anidado.** En cada servidor, la creación de reflejo bidireccional proporciona resistencia local y, a continuación, se proporciona una mayor resistencia a través de la creación de reflejo bidireccional entre los dos servidores. Es esencialmente un reflejo de cuatro vías, con dos copias en cada servidor. La creación de reflejo bidireccional anidada proporciona un rendimiento no efectivo: las escrituras van a todas las copias y las lecturas proceden de cualquier copia.

  ![Reflejo doble anidado](media/nested-resiliency/nested-two-way-mirror.png)

- **Paridad anidada con aceleración de reflejo.** Combina la creación de reflejo bidireccional anidada, anterior, con paridad anidada. Dentro de cada servidor, la resistencia local para la mayoría de los datos se proporciona mediante aritmética única de [paridad bit a bit](storage-spaces-fault-tolerance.md#parity), excepto las nuevas escrituras recientes que usan la creación de reflejo bidireccional. A continuación, la creación de reflejo bidireccional entre los servidores proporciona una mayor resistencia para todos los datos. Para obtener más información acerca de cómo funciona la paridad con aceleración de reflejo, consulte [paridad acelerada de reflejo](https://docs.microsoft.com/windows-server/storage/refs/mirror-accelerated-parity).

  ![Paridad anidada-con aceleración de reflejo](media/nested-resiliency/nested-mirror-accelerated-parity.png)

### <a name="capacity-efficiency"></a>Eficiencia de la capacidad

La eficacia de la capacidad es la proporción de espacio utilizable para la [superficie del volumen](plan-volumes.md#choosing-the-size-of-volumes). Describe la sobrecarga de capacidad atribuible a la resistencia y depende de la opción de resistencia que elija. Como ejemplo sencillo, el almacenamiento de datos sin resistencia es un 100% de capacidad eficaz (1 TB de datos ocupa 1 TB de capacidad de almacenamiento físico), mientras que la creación de reflejo bidireccional es un 50% eficaz (1 TB de datos ocupa 2 TB de capacidad de almacenamiento físico).

- **El reflejo bidireccional anidado** escribe cuatro copias de todo, lo que significa que se van a almacenar 1 TB de datos, se necesitan 4 TB de capacidad de almacenamiento físico. Aunque su simplicidad es atractiva, la eficacia de la capacidad del reflejo bidireccional anidada del 25% es el menor de cualquier opción de resistencia en Espacios de almacenamiento directo.

- La **paridad anidada con aceleración de reflejo** logra una mayor eficiencia de la capacidad, en torno al 35%-40%, que depende de dos factores: el número de unidades de capacidad en cada servidor y la combinación de reflejo y paridad que especifique para el volumen. En esta tabla se proporciona una búsqueda de configuraciones comunes:

  | Unidades de capacidad por servidor | 10% de reflejo | 20% de reflejo | un 30% de reflejo |
  |----------------------------|------------|------------|------------|
  | 4                          | 35,7%      | 34,1%      | 32,6%      |
  | 5                          | 37,7%      | 35,7%      | 33,9%      |
  | 6                          | 39,1%      | 36,8%      | 34,7%      |
  | 7 +                         | 40,0%      | 37,5%      | 35,3%      |

  > [!NOTE]
  > **Si tiene curiosidad, este es un ejemplo de las matemáticas completas.** Supongamos que tenemos seis unidades de capacidad en cada uno de dos servidores y queremos crear un volumen de 1 100 GB formado por 10 GB de reflejo y 90 GB de paridad. El reflejo bidireccional local del servidor es del 50,0% eficaz, lo que significa que los 10 GB de datos reflejados tardan 20 GB en almacenarse en cada servidor. Reflejado en ambos servidores, su superficie total es de 40 GB. La paridad única local del servidor, en este caso, es 5/6 = 83,3% eficaz, lo que significa que los 90 GB de datos de paridad toman 108 GB para almacenar en cada servidor. Reflejado en ambos servidores, su superficie total es de 216 GB. La superficie total es, por lo tanto [(10 GB/50,0%) + (90 GB/83,3%)] × 2 = 256 GB, para la eficacia de la capacidad total del 39,1%.

Observe que la eficacia de la capacidad del reflejo bidireccional clásico (aproximadamente 50%) y paridad anidada de reflejo (hasta 40%) no son muy diferentes. En función de sus requisitos, la eficacia de la capacidad ligeramente inferior puede merecer la pena el aumento significativo en la disponibilidad del almacenamiento. Elige resistencia por volumen, por lo que puede mezclar volúmenes de resistencia anidados y volúmenes de reflejo bidireccional clásicos en el mismo clúster.

![Equilibrio](media/nested-resiliency/tradeoff.png)

## <a name="usage-in-powershell"></a>Uso en PowerShell

Puede usar cmdlets de almacenamiento conocidos en PowerShell para crear volúmenes con resistencia anidada.

### <a name="step-1-create-storage-tier-templates"></a>Paso 1: creación de plantillas de capa de almacenamiento

En primer lugar, cree nuevas plantillas de capa de almacenamiento con el cmdlet `New-StorageTier`. Solo tiene que hacer esto una vez y cada nuevo volumen que cree puede hacer referencia a estas plantillas. Especifique el `-MediaType` de las unidades de capacidad y, opcionalmente, el `-FriendlyName` de su elección. No modifique los otros parámetros.

Si las unidades de capacidad son unidades de disco duro (HDD), inicie PowerShell como administrador y ejecute:

```PowerShell 
# For mirror
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedMirror -ResiliencySettingName Mirror -MediaType HDD -NumberOfDataCopies 4

# For parity
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedParity -ResiliencySettingName Parity -MediaType HDD -NumberOfDataCopies 2 -PhysicalDiskRedundancy 1 -NumberOfGroups 1 -FaultDomainAwareness StorageScaleUnit -ColumnIsolation PhysicalDisk 
``` 

Si las unidades de capacidad son unidades de estado sólido (SSD), establezca el `-MediaType` en `SSD` en su lugar. No modifique los otros parámetros.

> [!TIP]
> Compruebe que los niveles se crearon correctamente con `Get-StorageTier`.

### <a name="step-2-create-volumes"></a>Paso 2: creación de volúmenes

A continuación, cree nuevos volúmenes con el cmdlet `New-Volume`.

#### <a name="nested-two-way-mirror"></a>Reflejo doble anidado

Para usar un reflejo bidireccional anidado, haga referencia a la plantilla de nivel de `NestedMirror` y especifique el tamaño. Por ejemplo:

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume01 -StorageTierFriendlyNames NestedMirror -StorageTierSizes 500GB
```

#### <a name="nested-mirror-accelerated-parity"></a>Paridad anidada-con aceleración de reflejo

Para usar la paridad anidada con aceleración de reflejo, haga referencia a las plantillas de nivel `NestedMirror` y `NestedParity` y especifique dos tamaños, uno para cada parte del volumen (reflejo primero, paridad segundo). Por ejemplo, para crear un volumen de 1 500 GB que tenga un 20% de reflejo doble anidado y una paridad anidada del 80%, ejecute:

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume02 -StorageTierFriendlyNames NestedMirror, NestedParity -StorageTierSizes 100GB, 400GB
```

### <a name="step-3-continue-in-windows-admin-center"></a>Paso 3: continuar en el centro de administración de Windows

Los volúmenes que usan resistencia anidada aparecen en el [centro de administración de Windows](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center) con etiquetas claras, como se muestra en la captura de pantalla siguiente. Una vez creados, puede administrarlos y supervisarlos mediante el centro de administración de Windows como cualquier otro volumen en Espacios de almacenamiento directo.

![](media/nested-resiliency/windows-admin-center.png)

### <a name="optional-extend-to-cache-drives"></a>Opcional: extender a unidades de caché

Con su configuración predeterminada, la resistencia anidada protege frente a la pérdida de varias unidades de capacidad al mismo tiempo, o de una unidad de servidor y de capacidad al mismo tiempo. Para ampliar esta protección a las [unidades de caché](understand-the-cache.md) se debe tener en cuenta lo siguiente: dado que las unidades de caché proporcionan a menudo almacenamiento en caché de lectura *y escritura* para unidades de *varias* capacidades, la única manera de asegurarse de que puede tolerar la pérdida de una unidad de caché cuando el otro servidor está inactivo es simplemente no almacenar en caché escrituras, pero esto afecta al rendimiento.

Para abordar este escenario, Espacios de almacenamiento directo ofrece la opción de deshabilitar automáticamente el almacenamiento en caché de escritura cuando un servidor de un clúster de dos servidores está inactivo y volver a habilitar el almacenamiento en caché de escritura una vez que el servidor se copia de seguridad. Para permitir el reinicio de rutina sin impacto en el rendimiento, el almacenamiento en caché de escritura no está deshabilitado hasta que el servidor ha estado inactivo durante 30 minutos. Una vez deshabilitado el almacenamiento en caché de escritura, el contenido de la memoria caché de escritura se escribe en los dispositivos de capacidad. Después, el servidor puede tolerar un error en el dispositivo de caché en el servidor en línea, aunque las lecturas de la memoria caché se pueden retrasar o generar un error si se produce un error en un dispositivo de caché.

Para establecer este comportamiento (opcional), inicie PowerShell como administrador y ejecute:

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.NestedResiliency.DisableWriteCacheOnNodeDown.Enabled" -Value "True"
```

Una vez que se establece en **true**, el comportamiento de la memoria caché es:

| Problema                       | Comportamiento de caché                           | ¿Puede tolerar la pérdida de la unidad de caché? |
|---------------------------------|------------------------------------------|--------------------------------|
| Ambos servidores en vertical                 | Lecturas y escrituras en caché, rendimiento completo | Sí                            |
| Servidor inactivo, primeros 30 minutos   | Lecturas y escrituras en caché, rendimiento completo | No (temporalmente)               |
| Después de los primeros 30 minutos          | Solo lecturas de caché, impacto en el rendimiento   | Sí (después de que la memoria caché se haya escrito en unidades de capacidad)                           |

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="can-i-convert-an-existing-volume-between-two-way-mirror-and-nested-resiliency"></a>¿Se puede convertir un volumen existente entre reflejo doble y resistencia anidada?

No, los volúmenes no se pueden convertir entre tipos de resistencia. En el caso de las nuevas implementaciones en Windows Server 2019, decida con anterioridad el tipo de resistencia que mejor se adapte a sus necesidades. Si va a actualizar desde Windows Server 2016, puede crear nuevos volúmenes con resistencia anidada, migrar los datos y, a continuación, eliminar los volúmenes más antiguos.

### <a name="can-i-use-nested-resiliency-with-multiple-types-of-capacity-drives"></a>¿Puedo usar la resistencia anidada con varios tipos de unidades de capacidad?

Sí, solo tiene que especificar el `-MediaType` de cada nivel como corresponda durante el [paso 1](#step-1-create-storage-tier-templates) anterior. Por ejemplo, con NVMe, SSD y HDD en el mismo clúster, NVMe proporciona memoria caché mientras que las dos últimas proporcionan capacidad: establezca el nivel de `NestedMirror` en `-MediaType SSD` y el nivel de `NestedParity` en `-MediaType HDD`. En este caso, tenga en cuenta que la eficacia de la capacidad de paridad depende solo del número de unidades HDD y necesita al menos 4 de ellas por servidor.

### <a name="can-i-use-nested-resiliency-with-3-or-more-servers"></a>¿Puedo usar la resistencia anidada con 3 o más servidores?

No, usar solo la resistencia anidada si el clúster tiene exactamente 2 servidores.

### <a name="how-many-drives-do-i-need-to-use-nested-resiliency"></a>¿Cuántas unidades necesito para usar la resistencia anidada?

El número mínimo de unidades necesarias para Espacios de almacenamiento directo es de 4 unidades de capacidad por nodo de servidor, además de 2 unidades de caché por nodo de servidor (si existe). Esto no ha cambiado en Windows Server 2016. No hay ningún requisito adicional para la resistencia anidada y la recomendación para la capacidad de reserva no cambia también.

### <a name="does-nested-resiliency-change-how-drive-replacement-works"></a>¿Cambia la resistencia anidada cómo funciona la sustitución de la unidad?

No.

### <a name="does-nested-resiliency-change-how-server-node-replacement-works"></a>¿Cambia la resistencia anidada el modo en que funciona el reemplazo del nodo de servidor?

No. Para reemplazar un nodo de servidor y sus unidades, siga este orden:

1. Retirar las unidades del servidor de salida
2. Agregar el nuevo servidor, con sus unidades, al clúster
3. El bloque de almacenamiento se reequilibrará
4. Quitar el servidor de salida y sus unidades

Para obtener más información, consulte el tema [quitar servidores](remove-servers.md) .

## <a name="see-also"></a>Vea también

- [Información general de Espacios de almacenamiento directo](storage-spaces-direct-overview.md)
- [Comprender la tolerancia a errores en Espacios de almacenamiento directo](storage-spaces-fault-tolerance.md)
- [Planear volúmenes en Espacios de almacenamiento directo](plan-volumes.md)
- [Crear volúmenes en Espacios de almacenamiento directo](create-volumes.md)

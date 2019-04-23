---
title: Resistencia anidada de espacios de almacenamiento directo
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dansimp
ms.technology: storagespaces
ms.topic: article
author: cosmosdarwin
ms.date: 11/06/2018
ms.openlocfilehash: 206d5d19ec55774f9055e7265d4c87d276c60590
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879576"
---
# <a name="nested-resiliency-for-storage-spaces-direct"></a>Resistencia anidada de espacios de almacenamiento directo

> Se aplica a: Windows Server 2019

Resistencia anidada es una nueva funcionalidad de [espacios de almacenamiento directo](storage-spaces-direct-overview.md) en Windows Server 2019 que permite a un clúster de dos servidores resistir los errores de hardware al mismo tiempo sin pérdida de disponibilidad de almacenamiento, para que los usuarios, aplicaciones, y las máquinas virtuales continúan ejecutándose sin interrupción. En este tema se explica cómo funciona, proporciona instrucciones paso a paso para empezar a trabajar y respuestas más preguntas más frecuentes.

## <a name="prerequisites"></a>Requisitos previos

### <a name="green-checkmark-iconmedianested-resiliencysupportedpng-consider-nested-resiliency-if"></a>![Icono de marca de verificación verde.](media/nested-resiliency/supported.png) Considere la posibilidad de resistencia anidada si:

- El clúster ejecuta Windows Server 2019; y
- El clúster tiene exactamente 2 nodos de servidor

### <a name="red-x-iconmedianested-resiliencyunsupportedpng-you-cant-use-nested-resiliency-if"></a>![Icono X rojo.](media/nested-resiliency/unsupported.png) No puede usar la resistencia anidada si:

- El clúster ejecuta Windows Server 2016; o
- El clúster tiene 3 o más nodos de servidor

## <a name="why-nested-resiliency"></a>¿Por qué resistencia anidado

Volúmenes que usen resistencia anidada pueden **permanezca en línea y accesibles, incluso si se producen varios errores de hardware al mismo tiempo**, a diferencia de classic [la creación de reflejo bidireccional](storage-spaces-fault-tolerance.md) resistencia. Por ejemplo, si se producen errores de dos unidades al mismo tiempo, o si un servidor deja de funcionar y se produce un error en una unidad, volúmenes que usen resistencia anidada permanecen en línea y accesibles. Para las infraestructuras hiperconvergidas, esto aumenta el tiempo de actividad para las aplicaciones y máquinas virtuales; para cargas de trabajo de servidor de archivos, esto significa que los usuarios disfrutan de un acceso ininterrumpido a sus archivos.

![Disponibilidad de almacenamiento](media/nested-resiliency/storage-availability.png)

El inconveniente es que resistencia anidada tiene **reducir la eficacia de la capacidad de creación de reflejo de bidireccional clásico**, lo que significa que obtendrá un poco menos espacio utilizable. Para obtener más información, consulte el [la eficacia de la capacidad](#capacity-efficiency) sección más adelante.

## <a name="how-it-works"></a>Cómo funciona

### <a name="inspiration-raid-51"></a>Inspiración: RAID 5 + 1

RAID 5 + 1 es un formulario establecido de resistencia de almacenamiento distribuido que proporciona información útil para resistencia anidadas de descripción. En RAID 5 + 1, dentro de cada servidor, resistencia local se proporciona mediante RAID-5, o *único paridad*, para protegerse contra la pérdida de cualquier unidad individual. A continuación, más RAID-1, proporciona resistencia o *la creación de reflejo bidireccional*, entre los dos servidores para protegerse contra la pérdida de cualquier servidor.

![RAID 5 + 1](media/nested-resiliency/raid-51.png)

### <a name="two-new-resiliency-options"></a>Dos nuevas opciones de resistencia

Espacios de almacenamiento directo en Windows Server 2019 ofrece dos nuevas opciones de resistencia, implementadas en el software, sin necesidad de hardware especializado de RAID:

- **Reflejo doble anidada.** Dentro de cada servidor, se proporciona resistencia local mediante la creación de reflejo bidireccional y, a continuación, se proporciona más resistencia mediante la creación de reflejo bidireccional entre los dos servidores. Es esencialmente un espejo de cuatro vías, con dos copias de cada servidor. Creación de reflejo de bidireccional anidada proporciona un rendimiento excepcional: escrituras vaya a todas las copias y lecturas proceden de cualquier copia.

  ![Reflejo doble anidado](media/nested-resiliency/nested-two-way-mirror.png)

- **Paridad acelerada reflejado anidado.** Combinar anidados de creación de reflejo bidireccional, de lo anterior, con la paridad anidado. Dentro de cada servidor, solo proporciona resistencia local para la mayoría de los datos [paridad de bit a bit aritmético](storage-spaces-fault-tolerance.md#parity), excepto nuevas escrituras recientes que usan la creación de reflejo bidireccional. A continuación, aún más resistencia para todos los datos se proporciona mediante la creación de reflejo bidireccional entre los servidores. Para obtener más información acerca de cómo reflejado acelerada paridad funciona, consulte [acelerada reflejado paridad](https://docs.microsoft.com/windows-server/storage/refs/mirror-accelerated-parity).

  ![Paridad acelerada reflejado anidado](media/nested-resiliency/nested-mirror-accelerated-parity.png)

### <a name="capacity-efficiency"></a>Eficacia de capacidad

Eficacia de la capacidad es la proporción de espacio utilizable para [superficie volumen](plan-volumes.md#choosing-the-size-of-volumes). Describe la sobrecarga de capacidad atribuible a la resistencia y depende de la opción de resistencia que elija. Como ejemplo sencillo, almacenamiento de datos sin resistencia es la capacidad del 100% eficaz (1 TB de datos ocupa 1 TB de capacidad de almacenamiento físico), mientras que la creación de reflejo bidireccional es 50% eficaz (de 1 TB de datos tarda hasta 2 TB de capacidad de almacenamiento físico).

- **Reflejo doble anidada** escribe cuatro copias de todo, lo que significa que para el almacenamiento de 1 TB de datos, necesita 4 TB de capacidad de almacenamiento físico. Aunque su simplicidad es muy atractivo, la eficacia de la capacidad de anidada bidireccional del reflejo del 25% es el más bajo de cualquier opción de resistencia en espacios de almacenamiento directo.

- **Anidar acelerada reflejado paridad** consigue mayor eficacia de capacidad, aproximadamente el 35% - 40%, que depende de dos factores: el número de capacidad de unidades en cada servidor y la combinación de reflejo y paridad que especifique para el volumen. Esta tabla proporciona una búsqueda de configuraciones comunes:

  | Unidades de capacidad por servidor | 10% reflejado | 20% reflejado | 30% reflejado |
  |----------------------------|------------|------------|------------|
  | 4                          | 35.7%      | 34.1%      | 32.6%      |
  | 5                          | 37.7%      | 35.7%      | 33.9%      |
  | 6                          | 39.1%      | 36.8%      | 34.7%      |
  | 7+                         | 40.0%      | 37.5%      | 35.3%      |

  > [!NOTE]
  > **Si tiene curiosidad, este es un ejemplo de la expresión matemática completa.** Supongamos que tenemos seis unidades de capacidad en cada uno de los dos servidores, y queremos crear un volumen de 100 GB que consta de 10 GB de reflejo y 90 GB de paridad. Reflejo doble de servidor local es 50,0% eficaz, lo que significa que los 10 GB de datos reflejada tarda 20 GB para almacenar en cada servidor. Se refleja en ambos servidores, su superficie total es de 40 GB. Servidor local de paridad única, en este caso, es 5/6 = 83.3% eficaz, lo que significa que el 90 GB de datos de paridad toma 108 GB para almacenar en cada servidor. Se refleja en ambos servidores, su superficie total es 216 GB. Por lo tanto, la superficie total [(10 GB/50,0%) + (90 GB / 83.3%)] × 2 = 256 GB, para 39,1% general la eficacia de la capacidad.

Tenga en cuenta que la eficacia de la capacidad de recursos clásicos (aproximadamente el 50%) de la creación de reflejo bidireccional y anidar acelerada reflejado paridad (hasta un 40%) no son muy diferentes. Según sus requisitos, puede ser la eficacia de capacidad ligeramente inferior que merece la pena el significativo aumento de disponibilidad de almacenamiento. Elija resistencia por volumen, por lo que puede mezclar los volúmenes de resistencia anidados y clásico reflejado bidireccional dentro del mismo clúster.

![Tradeoff](media/nested-resiliency/tradeoff.png)

## <a name="usage-in-powershell"></a>Uso en PowerShell

Puede usar cmdlets de almacenamiento conocidas en PowerShell para crear volúmenes con resistencia anidado.

### <a name="step-1-create-storage-tier-templates"></a>Paso 1: Crear plantillas de nivel de almacenamiento

En primer lugar, crear nuevas plantillas de nivel de almacenamiento mediante el `New-StorageTier` cmdlet. Basta con hacerlo una vez y, a continuación, cada volumen nuevo creado puede hacer referencia a estas plantillas. Especifique el `-MediaType` de las unidades de capacidad y, opcionalmente, el `-FriendlyName` de su elección. No modifique los demás parámetros.

Si las unidades de capacidad son unidades de disco duro (HDD), inicie PowerShell como administrador y ejecute:

```PowerShell 
# For mirror
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedMirror -ResiliencySettingName Mirror -MediaType HDD -NumberOfDataCopies 4

# For parity
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedParity -ResiliencySettingName Parity -MediaType HDD -NumberOfDataCopies 2 -PhysicalDiskRedundancy 1 -NumberOfGroups 1 -FaultDomainAwareness StorageScaleUnit -ColumnIsolation PhysicalDisk 
``` 

Si las unidades de capacidad unidades de estado sólido (SSD), establezca el `-MediaType` a `SSD` en su lugar. No modifique los demás parámetros.

> [!TIP]
> Compruebe los niveles que se creó correctamente con `Get-StorageTier`.

### <a name="step-2-create-volumes"></a>Paso 2: Crear volúmenes

A continuación, crear nuevos volúmenes mediante el `New-Volume` cmdlet.

#### <a name="nested-two-way-mirror"></a>Reflejo doble anidado

Para usar anidada reflejado bidireccional, hacer referencia a la `NestedMirror` capa de plantilla y especifique el tamaño. Por ejemplo:

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume01 -StorageTierFriendlyNames NestedMirror -StorageTierSizes 500GB
```

#### <a name="nested-mirror-accelerated-parity"></a>Paridad acelerada reflejado anidado

Para usar la paridad acelerada reflejado anidado, hacer referencia tanto la `NestedMirror` y `NestedParity` plantillas de nivel y especificar los tamaños de dos, uno para cada parte del volumen (crear el reflejo en primer lugar, paridad en segundo lugar). Por ejemplo crear un volumen de 500 GB es reflejado bidireccional anidada de un 20% y 80% anidado paridad, ejecute:

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume02 -StorageTierFriendlyNames NestedMirror, NestedParity -StorageTierSizes 100GB, 400GB
```

### <a name="step-3-continue-in-windows-admin-center"></a>Paso 3: Continuar en Windows Admin Center

Volúmenes que usen resistencia anidada aparecen en [Windows Admin Center](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center) con etiquetado claro, como se muestra en la captura de pantalla siguiente. Una vez que se creen, puede administrar y supervisar mediante Windows Admin Center al igual que cualquier otro volumen en espacios de almacenamiento directo.

![](media/nested-resiliency/windows-admin-center.png)

### <a name="optional-extend-to-cache-drives"></a>Opcional: Extender a unidades de caché

Con sus valores predeterminados, resistencia anidada protege contra la pérdida de varias unidades de capacidad al mismo tiempo, o un servidor y una unidad de capacidad al mismo tiempo. Para ampliar esta protección a [unidades de caché](understand-the-cache.md) tiene una consideración adicional: dado que las unidades de caché suelen ofrecen lectura *y escribir* almacenamiento en caché para *varios* unidades de capacidad , la única manera de asegurarse de que puede tolerar la pérdida de una unidad de caché cuando el otro servidor esté inactivo es simplemente no almacenar en caché escrituras, pero que afecta al rendimiento.

Para solucionar este escenario, espacios de almacenamiento directo ofrece la opción de deshabilitar automáticamente la memoria caché de escritura cuando un servidor en un clúster de dos servidores está inactivo y, a continuación, volver a habilitar una vez que esté el servidor de copia de seguridad de la caché de escritura. Para permitir los reinicios rutinarios sin afectar al rendimiento, escribir el almacenamiento en caché no está deshabilitado hasta que el servidor ha estado inactivo durante 30 minutos.

Para establecer este comportamiento (opcional), inicie PowerShell como administrador y ejecute:

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.NestedResiliency.DisableWriteCacheOnNodeDown.Enabled" -Value "True"
```

Una vez establecido en `True`, es el comportamiento de caché:

| Situación                       | Comportamiento de la caché                           | ¿Puede tolerar la pérdida de la unidad de memoria caché? |
|---------------------------------|------------------------------------------|--------------------------------|
| Ambos servidores de seguridad                 | Caché de lectura y escritura, un rendimiento completo | Sí                            |
| Servidor inactivo, los primeros 30 minutos   | Caché de lectura y escritura, un rendimiento completo | No (temporalmente)               |
| Después de los primeros 30 minutos          | Lecturas de caché solo, el rendimiento afectado   | Sí                            |

## <a name="frequently-asked-questions"></a>Preguntas frecuentes

### <a name="can-i-convert-an-existing-volume-between-two-way-mirror-and-nested-resiliency"></a>¿Puedo convertir un volumen existente entre reflejo doble y la resistencia anidado?

No, los volúmenes no se puede convertir entre tipos de resistencia. Para las nuevas implementaciones de Windows Server 2019, decidir antemano qué tipo de resistencia que mejor se adapte a sus necesidades. Si va a actualizar desde Windows Server 2016, puede crear nuevos volúmenes con resistencia anidado, migrar los datos y, a continuación, elimine los volúmenes anteriores.

### <a name="can-i-use-nested-resiliency-with-multiple-types-of-capacity-drives"></a>¿Puedo usar resistencia anidado con varios tipos de unidades de capacidad?

Sí, solo hay que especificar el `-MediaType` de cada nivel según corresponda durante [Paso1](#step-1-create-storage-tier-templates) anteriormente. Por ejemplo, con NVMe, SSD y HDD en el mismo clúster, el NVMe proporciona memoria caché mientras que los dos últimos proporcionan una capacidad: establecer el `NestedMirror` nivel para `-MediaType SSD` y `NestedParity` nivel para `-MediaType HDD`. En este caso, tenga en cuenta que la eficiencia de paridad capacidad depende del número de unidades de disco duro solamente, y necesita al menos 4 de ellas por servidor.

### <a name="can-i-use-nested-resiliency-with-3-or-more-servers"></a>¿Puedo usar resistencia anidado con 3 o más servidores?

No, utilizar resistencia anidada sólo si el clúster tiene exactamente 2 servidores.

### <a name="how-many-drives-do-i-need-to-use-nested-resiliency"></a>¿Cuántas unidades es necesario usar anidada resistencia?

El número mínimo de unidades necesarias para espacios de almacenamiento directo es de 4 unidades de capacidad por nodo de servidor, además de las unidades de caché de 2 por nodo de servidor (si existe). Esto se ha cambiado desde Windows Server 2016. No hay ningún requisito adicional para proporcionar resistencia anidada y la recomendación para la capacidad de reserva también se modifica.

### <a name="does-nested-resiliency-change-how-drive-replacement-works"></a>¿Anidada resistencia cambia el funcionamiento de la sustitución de la unidad?

No.

### <a name="does-nested-resiliency-change-how-server-node-replacement-works"></a>¿Resistencia anidada cambia cómo funciona el reemplazo del nodo de servidor?

No. Para reemplazar un nodo de servidor y sus unidades, siga este orden:

1. Retirar las unidades en el servidor de salida
2. Agregue un nuevo servidor, con sus unidades, en el clúster
3. El bloque de almacenamiento reequilibrará
4. Quitar el servidor de salida y sus unidades

Para obtener información detallada, consulte el [quitar servidores](remove-servers.md) tema.

## <a name="see-also"></a>Vea también

- [Información general de espacios directo de almacenamiento](storage-spaces-direct-overview.md)
- [Comprender la tolerancia a errores en los espacios de almacenamiento directo](storage-spaces-fault-tolerance.md)
- [Plan de volúmenes en espacios de almacenamiento directo](plan-volumes.md)
- [Crear volúmenes en espacios de almacenamiento directo](create-volumes.md)

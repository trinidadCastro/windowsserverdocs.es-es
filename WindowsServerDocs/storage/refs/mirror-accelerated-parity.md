---
description: Más información acerca de la paridad de reflejo acelerado
title: Paridad acelerada por reflejo
ms.author: gawatu
manager: masriniv
ms.topic: article
author: gawatu
ms.date: 10/17/2018
ms.assetid: ''
ms.openlocfilehash: b39e3d518b3721bffce7b111655406cd982ccc0b
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97043603"
---
# <a name="mirror-accelerated-parity"></a>Paridad acelerada por reflejo

>Se aplica a: Windows Server 2019, Windows Server 2016

Los espacios de almacenamiento pueden proporcionar tolerancia a errores para los datos mediante dos técnicas fundamentales: reflejo y paridad. En [espacios de almacenamiento directo](../storage-spaces/storage-spaces-direct-overview.md), ReFS introduce una paridad acelerada para el reflejo, que permite crear volúmenes que usan resistencias de reflejo y paridad. La paridad con aceleración de reflejo ofrece almacenamiento económico y rentable sin sacrificar el rendimiento.

![Mirror-Accelerated-Parity-Volume](media/mirror-accelerated-parity/Mirror-Accelerated-Parity-Volume.png)

## <a name="background"></a>Información previa

Los esquemas de resistencia de reflejo y paridad tienen características de almacenamiento y rendimiento fundamentalmente diferentes:
- La resistencia reflejada permite a los usuarios lograr un rendimiento de escritura rápido, pero la replicación de los datos de cada copia no es eficaz.
- La paridad, por otro lado, debe volver a calcular la paridad para cada escritura, lo que provocará que el rendimiento de escritura aleatorio se vea afectado. Sin embargo, la paridad permite que los usuarios almacenen sus datos con mayor eficacia de espacio. Para obtener más información, consulte [tolerancia a errores de espacios de almacenamiento](../storage-spaces/Storage-Spaces-Fault-Tolerance.md).

Por lo tanto, el reflejo se ha desechado para ofrecer almacenamiento sensible al rendimiento, mientras que la paridad ofrece una mejor utilización de la capacidad de almacenamiento. En la paridad con aceleración de reflejo, ReFS aprovecha las ventajas de cada tipo de resistencia para ofrecer almacenamiento de gran capacidad y sensible al rendimiento mediante la combinación de los esquemas de resistencia de un solo volumen.

## <a name="data-rotation-on-mirror-accelerated-parity"></a>Rotación de datos en paridad de reflejo acelerado

ReFS gira activamente los datos entre el reflejo y la paridad, en tiempo real. Esto permite escribir rápidamente las escrituras entrantes en el reflejo y luego girarlas a la paridad para que se almacenen de forma eficaz. Al hacerlo, la e/s de entrada se presta más rápido en el reflejo, mientras que los datos inactivos se almacenan de forma eficaz en paridad, lo que proporciona un rendimiento óptimo y almacenamiento de costos perdidos en el mismo volumen.

Para girar datos entre el reflejo y la paridad, ReFS divide lógicamente el volumen en regiones de 64 MiB, que son la unidad de rotación. En la imagen siguiente se muestra un volumen de paridad acelerado para reflejo dividido en regiones.

![Mirror-Accelerated-Parity-Volume-with-Storage-containers](media/mirror-accelerated-parity/Mirror-Accelerated-Parity-Volume-with-Storage-Containers.png)

ReFS comienza a rotar las regiones completas de reflejo a paridad una vez que el nivel de reflejo ha alcanzado un nivel de capacidad especificado. En lugar de mover inmediatamente los datos del reflejo a la paridad, ReFS espera y retiene los datos en el reflejo siempre que sea posible, lo que permite que ReFS siga proporcionando un rendimiento óptimo para los datos (vea "rendimiento de e/s" a continuación).

Cuando los datos se mueven del reflejo a la paridad, se leen los datos, se calculan las codificaciones de paridad y, a continuación, los datos se escriben en la paridad. La animación siguiente muestra esto con una región reflejada triple que se convierte en una región codificada de borrado durante la rotación:

![Reflejo-acelerado-paridad-rotación](media/mirror-accelerated-parity/Container-Rotation.gif)

## <a name="io-on-mirror-accelerated-parity"></a>E/s en paridad acelerada de reflejo
### <a name="io-behavior"></a>Comportamiento de e/s
**Escrituras:** ReFS Services entrantes escrituras de tres maneras distintas:

1.  **Escribe en el reflejo:**

    - **1Una.** Si la escritura entrante modifica los datos existentes en el reflejo, ReFS modificará los datos en su lugar.
    - **ter.** Si la escritura entrante es una nueva escritura y ReFS puede encontrar correctamente suficiente espacio disponible en el reflejo para atender esta escritura, ReFS escribirá en el reflejo.
    ![Escritura en reflejo](media/mirror-accelerated-parity/Write-to-Mirror.png)

2. **Escribe en el reflejo, reasignado desde la paridad:**

    Si la escritura entrante modifica los datos que están en paridad y ReFS puede encontrar correctamente suficiente espacio disponible en el reflejo para atender la escritura entrante, ReFS invalidará primero los datos anteriores en paridad y, a continuación, escribirá en el reflejo. Esta invalidación es una operación de metadatos rápida y económica que ayuda a mejorar de forma significativa el rendimiento de escritura realizado en la paridad.
    ![Reasignado: escritura](media/mirror-accelerated-parity/Reallocated-Write.png)

3. **Escribe en la paridad:**

    Si ReFS no puede encontrar correctamente suficiente espacio disponible en el reflejo, ReFS escribirá nuevos datos en la paridad o modificará los datos existentes en la paridad directamente. En la sección "optimizaciones de rendimiento" se proporcionan instrucciones que ayudan a minimizar las escrituras en la paridad.
    ![Escritura a paridad](media/mirror-accelerated-parity/Write-to-Parity.png)

**Lecturas:** ReFS leerá directamente desde el nivel que contiene los datos pertinentes. Si la paridad se construye con HDD, la memoria caché de Espacios de almacenamiento directo almacenará en caché estos datos para acelerar las lecturas futuras.

> [!NOTE]
> Las lecturas nunca provocan que las referencias vuelvan a girar los datos en el nivel de reflejo.

### <a name="io-performance"></a>Rendimiento de e/s

**Escrituras:** Cada tipo de escritura descrito anteriormente tiene sus propias características de rendimiento. En términos generales, las escrituras en el nivel de reflejo son mucho más rápidas que las escrituras reasignadas y las escrituras reasignadas son significativamente más rápidas que las escrituras realizadas directamente al nivel de paridad. Esta relación se muestra en la desigualdad siguiente:


- **Nivel de reflejo > escrituras reasignadas >> nivel de paridad**


**Lecturas:** No hay ningún impacto negativo significativo en el rendimiento al leer de la paridad:
- Si el reflejo y la paridad se construyen con el mismo tipo de medio, el rendimiento de lectura será equivalente.
- Si el reflejo y la paridad se construyen con distintos tipos de medios (SSD reflejadas, HDD de paridad, por ejemplo),[la memoria caché de espacios de almacenamiento directo](../storage-spaces/understand-the-cache.md) ayudará a almacenar en caché los datos activos para acelerar las lecturas de la paridad.

## <a name="refs-compaction"></a>Compactación de ReFS

En esta versión semestral, ReFS incorpora compactación, lo que mejora considerablemente el rendimiento de los volúmenes de paridad acelerados para reflejos que están llenos en el 90%.

**Información general:** Anteriormente, a medida que los volúmenes de paridad acelerada de reflejo se llenaban, el rendimiento de estos volúmenes podía degradarse. El rendimiento disminuye porque los datos activos y en frío se mezclan a lo largo de las horas extras del volumen. Esto significa que los datos inactivos se pueden almacenar en reflejo, ya que los datos inactivos ocupan espacio en reflejo que, de otro modo, podrían usar los datos activos. Almacenar los datos activos en reflejo es fundamental para mantener un alto rendimiento, ya que las escrituras directamente en el reflejo son mucho más rápidas que las escrituras reasignadas y los pedidos de magnitud más rápido que las escrituras directamente en la paridad. Por lo tanto, tener los datos inactivos en el reflejo es incorrecto para el rendimiento, ya que reduce la probabilidad de que ReFS pueda realizar escrituras directamente en el reflejo.

La compactación ReFS soluciona estos problemas de rendimiento liberando espacio en el reflejo de los datos activos. En primer lugar, la compactación consolida todos los datos (desde el reflejo y la paridad) en la paridad. Esto reduce la fragmentación dentro del volumen y aumenta la cantidad de espacio direccionable en el reflejo. Y lo que es más importante, este proceso permite a las referencias consolidar los datos activos en el servidor reflejado:
-   Cuando llegan nuevas escrituras, se les presta servicio en el reflejo. Por lo tanto, los datos activos recién escritos se encuentran en el reflejo.
-   Cuando se realiza una escritura modificada en los datos en la paridad, ReFS realiza una escritura reasignada, por lo que esta escritura también se da servicio en el reflejo. Por lo tanto, los datos activos que se movieron a la paridad durante la compactación se volverán a asignar en reflejo.

## <a name="performance-optimizations"></a>Optimizaciones de rendimiento

>[!IMPORTANT]
> Se recomienda colocar los VHD con mucha escritura en subdirectorios diferentes. Esto se debe a que ReFS escribe los cambios de metadatos en el nivel de un directorio y sus archivos. Por lo tanto, si distribuye archivos con mucha escritura a través de directorios, las operaciones de metadatos son más pequeñas y se ejecutan en paralelo, lo que reduce la latencia de las aplicaciones.

### <a name="performance-counters"></a>Contadores de rendimiento

ReFS mantiene los contadores de rendimiento para ayudar a evaluar el rendimiento de la paridad con aceleración del reflejo.
- Tal y como se describió anteriormente en la sección escritura en paridad, ReFS escribirá directamente en la paridad cuando no encuentre espacio disponible en el reflejo. Por lo general, esto ocurre cuando el nivel reflejado se llena más rápido que ReFS puede rotar los datos a la paridad. En otras palabras, la rotación de ReFS no puede mantener el ritmo de la ingesta. Los contadores de rendimiento que se indican a continuación identifican Cuándo ReFS escribe directamente en la paridad:

  ```
  # Windows Server 2016
  ReFS\Data allocations slow tier/sec
  ReFS\Metadata allocations slow tier/sec

  # Windows Server 2019
  ReFS\Allocation of Data Clusters on Slow Tier/sec
  ReFS\Allocation of Metadata Clusters on Slow Tier/sec
  ```

- Si estos contadores son distintos de cero, esto indica que ReFS no está girando los datos lo suficientemente rápido como para reflejarse. Para ayudar a mitigar esto, puede cambiar la intensidad de giro o aumentar el tamaño del nivel reflejado.

### <a name="rotation-aggressiveness"></a>Agresividad de giro

ReFS comienza a rotar los datos una vez que el reflejo ha alcanzado un umbral de capacidad especificado.
-   Los valores más altos de este umbral de rotación hacen que las referencias retengan los datos en el nivel de reflejo más tiempo. La salida de los datos activos en el nivel de reflejo es óptima para el rendimiento, pero ReFS no podrá atender eficazmente grandes cantidades de e/s entrantes.
-   Los valores inferiores permiten a ReFS desorganizar los datos de forma proactiva y mejorar la ingesta de entrada y salida. Esto se aplica a cargas de trabajo con una gran cantidad de ingesta, como el almacenamiento de archivo. Sin embargo, los valores inferiores podrían degradar el rendimiento de las cargas de trabajo de uso general. La rotación innecesaria de datos fuera del nivel de reflejo conlleva una reducción del rendimiento.

ReFS introduce un parámetro ajustable para ajustar este umbral, que se puede configurar mediante una clave del registro. Esta clave del registro debe configurarse en **cada nodo de una implementación de espacios de almacenamiento directo** y se requiere un reinicio para que los cambios surtan efecto.
-   **Clave:** HKEY_LOCAL_MACHINE\System\CurrentControlSet\Policies
-   **ValueName (DWORD):** DataDestageSsdFillRatioThreshold
-   **ValueType:** Proporción

Si no se establece esta clave del registro, ReFS usará un valor predeterminado de 85%.  Este valor predeterminado se recomienda para la mayoría de las implementaciones y no se recomiendan los valores por debajo del 50%. El siguiente comando de PowerShell muestra cómo establecer esta clave del registro con un valor de 75%:
```PowerShell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Policies -Name DataDestageSsdFillRatioThreshold -Value 75
 ```
 Para configurar esta clave del registro en cada nodo de una implementación de Espacios de almacenamiento directo, puede usar el siguiente comando de PowerShell:
 ```PowerShell
 $Nodes = 'S2D-01', 'S2D-02', 'S2D-03', 'S2D-04'
 Invoke-Command $Nodes {Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Policies -Name DataDestageSsdFillRatioThreshold -Value 75}
 ```

### <a name="increasing-the-size-of-the-mirrored-tier"></a>Aumentar el tamaño del nivel reflejado

Aumentar el tamaño del nivel reflejado permite a ReFS conservar una parte más grande del espacio de trabajo en el reflejo. Esto mejora la probabilidad de que ReFS pueda escribir directamente en el reflejo, lo que le ayudará a lograr un mejor rendimiento. Los cmdlets de PowerShell siguientes muestran cómo aumentar el tamaño del nivel reflejado:
```PowerShell
Resize-StorageTier -FriendlyName “Performance” -Size 20GB
Resize-StorageTier -InputObject (Get-StorageTier -FriendlyName “Performance”) -Size 20GB
```
>[!TIP]
>Asegúrese de cambiar el tamaño de la **partición** y el **volumen** después de cambiar el tamaño de **StorageTier**. Para obtener más información y ejemplos, consulte [cambiar el tamaño de los volúmenes](../storage-spaces/resize-volumes.md).

## <a name="creating-a-mirror-accelerated-parity-volume"></a>Creación de un volumen de paridad acelerado para reflejo

El siguiente cmdlet de PowerShell crea un volumen de paridad acelerada para el reflejo con una proporción de paridad: paridad de 20:80, que es la configuración recomendada para la mayoría de las cargas de trabajo. Para obtener más información y ejemplos, consulte [creación de volúmenes en espacios de almacenamiento directo](../storage-spaces/Create-volumes.md).

```PowerShell
New-Volume – FriendlyName “TestVolume” -FileSystem CSVFS_ReFS -StoragePoolFriendlyName “StoragePoolName” -StorageTierFriendlyNames Performance, Capacity -StorageTierSizes 200GB, 800GB
```

## <a name="additional-references"></a>Referencias adicionales

-   [Información general de ReFS](refs-overview.md)
-   [Clonación de bloques de ReFS](block-cloning.md)
-   [Flujos de integridad de ReFS](integrity-streams.md)
-   [Introducción a Espacios de almacenamiento directo](../storage-spaces/storage-spaces-direct-overview.md)

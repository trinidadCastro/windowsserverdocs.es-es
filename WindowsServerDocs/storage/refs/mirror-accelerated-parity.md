---
title: Paridad acelerada por reflejos
ms.prod: windows-server
ms.author: gawatu
ms.manager: masriniv
ms.technology: storage-file-systems
ms.topic: article
author: gawatu
ms.date: 10/17/2018
ms.assetid: ''
ms.openlocfilehash: 2721f1c744c5c03d8e4bce0508fd23fa5237f95f
ms.sourcegitcommit: 9a6a692a7b2a93f52bb9e2de549753e81d758d28
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2019
ms.locfileid: "72591094"
---
# <a name="mirror-accelerated-parity"></a>Paridad acelerada por reflejos

>Se aplica a: Windows Server 2019, Windows Server 2016

Los espacios de almacenamiento pueden proporcionar tolerancia a errores para datos mediante dos técnicas fundamentales: reflejos y paridad. En [Espacios de almacenamiento directo](../storage-spaces/storage-spaces-direct-overview.md), ReFS incluye la paridad acelerada por reflejos, lo que permite crear volúmenes que usan resistencias de reflejo y paridad. La paridad acelerada por reflejos ofrece almacenamiento económico que aprovecha el espacio sin sacrificar el rendimiento. 

![Mirror-Accelerated-Parity-Volume](media/mirror-accelerated-parity/Mirror-Accelerated-Parity-Volume.png)

## <a name="background"></a>Background

Los esquemas de resistencia de reflejo y paridad tienen distintas características de almacenamiento y rendimiento:
- La resistencia reflejada permite a los usuarios lograr un rendimiento de escritura rápido, pero la replicación de los datos de cada copia no es eficaz. 
- Por otra parte, la paridad debe volver a calcularse para cada escritura, lo que provoca un rendimiento aleatorio de la escritura. Sin embargo, la paridad permite a los usuarios almacenar sus datos con mayor eficacia de espacio. Para obtener más información, consulte [tolerancia a errores de espacios de almacenamiento](../storage-spaces/Storage-Spaces-Fault-Tolerance.md).

Por lo tanto, el reflejo debe estar predispuesto a ofrecer almacenamiento dependiente del rendimiento, mientras que la paridad ofrece un uso mejorado de la capacidad de almacenamiento. En la paridad acelerada por reflejos, ReFS saca partido de las ventajas de cada tipo de resistencia para proporcionar almacenamiento dependiente de la capacidad y del rendimiento mediante la combinación de ambos esquemas de resistencia en un único volumen.

## <a name="data-rotation-on-mirror-accelerated-parity"></a>Cambio de datos en la paridad acelerada por reflejos

ReFS cambia activamente datos entre el espejo y la paridad en tiempo real. Esto permite que las escrituras de entrada se escriban rápidamente en el reflejo y luego cambien a la paridad para almacenarse de forma eficaz. Al hacerlo, la E/S de entrada se realiza rápidamente en el reflejo mientras que los datos "fríos" se almacenan de forma eficaz en el reflejo, lo que ofrece un rendimiento óptimo y un almacenamiento rentable en el mismo volumen. 

Para cambiar los datos entre reflejo y paridad, ReFS divide lógicamente el volumen en regiones de 64 MiB, que es la unidad del cambio. La siguiente imagen ilustra un volumen de paridad acelerada por reflejos dividido en regiones. 

![Mirror-Accelerated-Parity-Volume-with-Storage-Containers](media/mirror-accelerated-parity/Mirror-Accelerated-Parity-Volume-with-Storage-Containers.png)

ReFS empieza a cambiar regiones completas de reflejo a paridad cuando el nivel de reflejo llega a un nivel de capacidad especificado. En lugar de trasladar inmediatamente los datos de reflejo paridad, ReFS espera y conserva los datos en el reflejo, siempre que sea posible, lo que permite a ReFS continuar ofreciendo un rendimiento óptimo para los datos (consulta "Rendimiento de E/S" a continuación). 

Cuando los datos se mueven desde el reflejo a la paridad, los datos se leen, las codificaciones de paridad se calculan y los datos se escriben en la paridad. La animación siguiente muestra cómo utilizar una región de reflejo triple que se convierte en una región de codificación de borrado durante el cambio:

![Mirror-Accelerated-Parity-Rotation](media/mirror-accelerated-parity/Container-Rotation.gif)

## <a name="io-on-mirror-accelerated-parity"></a>E/S en la paridad acelerada por reflejos
### <a name="io-behavior"></a>Comportamiento de E/S
**Escrituras:** ReFS ofrece escrituras de entrada de tres formas distintas:

1.  **Escribe en el reflejo:**

    - **1Una.** Si la escritura de entrada modifica los datos existentes en el reflejo, ReFS modificará los datos.
    - **ter.** Si la escritura de entrada es una nueva escritura y ReFS encuentra suficiente espacio libre en el reflejo para ofrecer esta escritura, ReFS escribirá en el reflejo.
    ](media/mirror-accelerated-parity/Write-to-Mirror.png) de ![Write a reflejo

2. **Escribe en el reflejo, reasignado desde la paridad:**

    Si la escritura entrante modifica los datos que están en paridad y ReFS puede encontrar correctamente suficiente espacio disponible en el reflejo para atender la escritura entrante, ReFS invalidará primero los datos anteriores en paridad y, a continuación, escribirá en el reflejo. Esta anulación es una operación de metadatos rápida y económica que ayuda considerablemente a mejorar el rendimiento de la escritura en la paridad.
    ](media/mirror-accelerated-parity/Reallocated-Write.png) ![Reallocated-Write

3. **Escribe en la paridad:**
    
    Si ReFS no puede encontrar suficiente espacio libre en el reflejo, ReFS escribirá nuevos datos en la paridad o modificará los datos existentes directamente en la paridad. En la sección "Optimizaciones de rendimiento" que aparece a continuación se proporcionan instrucciones que ayudan a minimizar las escrituras en la paridad.
    ](media/mirror-accelerated-parity/Write-to-Parity.png) de ![Write a paridad

**Lecturas:** ReFS leerá directamente desde el nivel que incluya los datos relevantes. Si la paridad se crea con unidades de disco duro (HDD), la memoria caché de los espacios de almacenamiento directo almacenará estos datos para acelerar lecturas futuras. 

> [!NOTE]
> Las lecturas nunca hacen que ReFS vuelva a cambiar datos en el nivel de reflejo. 

### <a name="io-performance"></a>Rendimiento de e/s

**Escrituras:** cada tipo de escritura que se ha descrito anteriormente tiene sus propias características de rendimiento. A grandes rasgos, las escrituras en el nivel de reflejo son mucho más rápidas que las escrituras reasignadas y las escrituras reasignadas son mucho más rápidas que las escrituras realizadas directamente en el nivel de paridad. Esta relación se muestra en la siguiente diferencia: 


- **Nivel de reflejo > escrituras reasignadas > nivel de paridad >**


**Lecturas:** no hay ningún impacto negativo en la lectura desde la paridad:
- Si el reflejo y la paridad se crean con el mismo tipo de medio, el rendimiento de la lectura será igual. 
- Si el reflejo y la paridad se crean con distintos tipos de medios, por ejemplo, SSD de espejo y HDD de paridad, [la memoria caché de los espacios de almacenamiento directo](../storage-spaces/understand-the-cache.md) permitirá que los datos "en caliente" aceleren las lecturas desde la paridad.

## <a name="refs-compaction"></a>Compactación de ReFS

En esta versión semestral, ReFS incorpora compactación, lo que mejora considerablemente el rendimiento de los volúmenes de paridad acelerados para reflejos que están llenos en el 90%. 

**En segundo plano:** dado que anteriormente los volúmenes de paridad acelerada por reflejos se llenaban, el rendimiento de estos volúmenes podía verse afectado. El rendimiento se ve afectado porque se mezclan datos "fríos" y "en caliente" durante las horas de extra del volumen. Esto significa que pueden almacenarse menos datos "en caliente" en el reflejo, ya que los datos "fríos" ocupan espacio en el reflejo que podrían utilizar los datos "en caliente". Almacenar datos "en caliente" en el reflejo es de vital importancia para mantener un alto nivel de rendimiento, ya que las escrituras directas al reflejo son mucho más rápidas que las escrituras reasignadas, así como las órdenes de magnitud son más rápidas que las escrituras directas a la paridad. Por lo tanto, tener datos "fríos" en el reflejo no es adecuado para el rendimiento, ya que reduce la probabilidad de que ReFS pueda escribir directamente en el reflejo. 

La compactación de ReFS aborda estos problemas de rendimiento, liberando espacio en el reflejo para datos "en caliente". La compactación consolida primero todos los datos, de reflejo y paridad, en la paridad. Esto reduce la fragmentación del volumen y aumenta la cantidad de espacio direccionable en el reflejo. Y lo que más importante, este proceso permite a ReFS volver a consolidar los datos "en caliente" en el reflejo:
-   Cuando se introducen nuevas escrituras, se proporcionarán en el reflejo. Por lo tanto, los nuevos datos escritos "en caliente" residen en el reflejo. 
-   Al modificar una escritura realizada en los datos de la paridad, ReFS realiza un escritura reasignada, así que dicha escritura también puede proporcionarse al reflejo. Por lo tanto, los datos "en caliente" que se movieron a la paridad durante la compactación se volverán a asignar al reflejo. 

## <a name="performance-optimizations"></a>Optimizaciones de rendimiento

>[!IMPORTANT]
> Se recomienda colocar los VHD con mucha escritura en subdirectorios diferentes. Esto se debe a que ReFS escribe los cambios de metadatos en el nivel de un directorio y sus archivos. Por lo tanto, si distribuye archivos con mucha escritura a través de directorios, las operaciones de metadatos son más pequeñas y se ejecutan en paralelo, lo que reduce la latencia de las aplicaciones.

### <a name="performance-counters"></a>Contadores de rendimiento

ReFS mantiene contadores de rendimiento para ayudar a evaluar el rendimiento de la paridad acelerada por reflejos. 
- Tal y como se describió anteriormente en la sección escritura en paridad, ReFS escribirá directamente en la paridad cuando no encuentre espacio disponible en el reflejo. Por lo general, esto se produce cuando el nivel de reflejo se llena de forma más rápida que ReFS puede cambiar los datos a la paridad. En otras palabras, el cambio de ReFS no es capaz de seguir el ritmo de la velocidad de ingesta. Los siguientes contadores de rendimiento identifican si ReFS escribe directamente en la paridad:

  ```
  # Windows Server 2016
  ReFS\Data allocations slow tier/sec
  ReFS\Metadata allocations slow tier/sec

  # Windows Server 2019
  ReFS\Allocation of Data Clusters on Slow Tier/sec
  ReFS\Allocation of Metadata Clusters on Slow Tier/sec
  ```

- Si estos contadores no están a cero, esto indica que ReFS no cambia los datos con la suficiente rapidez fuera del reflejo. Para ayudar a mitigar este problema, se puede cambiar la agresividad de cambio o aumentar el tamaño del nivel de reflejo.

### <a name="rotation-aggressiveness"></a>Agresividad de giro

ReFS empieza a cambiar datos cuando el reflejo llega a un umbral de capacidad especificado.
-   Los valores más altos de este umbral de cambio hacen que ReFS conserve los datos en el nivel de reflejo durante más tiempo. Dejar los datos "en caliente" en el nivel de reflejo es óptimo para el rendimiento, pero ReFS no podrá proporcionar de forma eficaz grandes cantidades de E/S de entrada. 
-   Los valores más bajos permiten que ReFS elimine copias intermedias de los datos y realice una mejor ingesta de E/S de entrada. Esto es aplica a las cargas de trabajo de ingesta intensiva, como el almacenamiento de archivos. Sin embargo, los valores más bajos, podrían afectar al rendimiento de las cargas de trabajo con fines generales. Cambiar de forma innecesaria los datos fuera del nivel de reflejo implica una disminución del rendimiento. 

ReFS incluye un parámetro ajustable para ajustar este umbral, que se puede configurar mediante una clave de registro. Esta clave de registro debe configurarse en **cada nodo de una implementación de espacios de almacenamiento directo**, por tanto, será necesario reiniciar para que los cambios entren en vigor. 
-   **Clave:** HKEY_LOCAL_MACHINE\System\CurrentControlSet\Policies
-   **ValueName (DWORD):** DataDestageSsdFillRatioThreshold
-   **ValueType:** porcentaje

Si no se establece esta clave de registro, ReFS usará un valor predeterminado de 85 %.  Se recomienda usar este valor predeterminado para la mayoría de las implementaciones y no se recomiendan los valores inferiores al 50 %. El siguiente comando de PowerShell muestra cómo establecer esta clave de registro con un valor de 75 %: 
```PowerShell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Policies -Name DataDestageSsdFillRatioThreshold -Value 75
 ```
 Para configurar esta clave de registro en cada nodo en una implementación de Espacios de almacenamiento directos, puedes usar el siguiente comando de PowerShell:
 ```PowerShell
 $Nodes = 'S2D-01', 'S2D-02', 'S2D-03', 'S2D-04'
 Invoke-Command $Nodes {Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Policies -Name DataDestageSsdFillRatioThreshold -Value 75}
 ```

### <a name="increasing-the-size-of-the-mirrored-tier"></a>Aumentar el tamaño del nivel reflejado

Aumentar el tamaño de la nivel de reflejo permite a ReFS conservar una mayor parte del conjunto de trabajo en el reflejo. Esto aumenta la probabilidad de que ReFS pueda escribir directamente en el reflejo, lo que ayudará a lograr un mejor rendimiento. Los siguientes cmdlets de PowerShell muestran cómo aumentar el tamaño del nivel de reflejo:
```PowerShell
Resize-StorageTier -FriendlyName “Performance” -Size 20GB
Resize-StorageTier -InputObject (Get-StorageTier -FriendlyName “Performance”) -Size 20GB
```
>[!TIP]
>Asegúrate de cambiar el tamaño de la **Partición** y **Volumen** después de cambiar el tamaño de **StorageTier**. Para obtener más información y ejemplos, consulta [Resize-Volumes](../storage-spaces/resize-volumes.md).

## <a name="creating-a-mirror-accelerated-parity-volume"></a>Crear un volumen de paridad acelerada por reflejos

El siguiente cmdlet de PowerShell crea un volumen paridad acelerada por reflejos con una relación Reflejo:Paridad de 20:80, que es la configuración recomendada para la mayoría de las cargas de trabajo. Para obtener más información y ejemplos, echa un vistazo a [Crear volúmenes en espacios de almacenamiento directo](../storage-spaces/Create-volumes.md).

```PowerShell
New-Volume – FriendlyName “TestVolume” -FileSystem CSVFS_ReFS -StoragePoolFriendlyName “StoragePoolName” -StorageTierFriendlyNames Performance, Capacity -StorageTierSizes 200GB, 800GB
```

## <a name="see-also"></a>Consulta también

-   [Información general sobre ReFS](refs-overview.md)
-   [Clonación de bloques de ReFS](block-cloning.md)
-   [Flujos de integridad de ReFS](integrity-streams.md)
-   [Información general de Espacios de almacenamiento directo](../storage-spaces/storage-spaces-direct-overview.md)

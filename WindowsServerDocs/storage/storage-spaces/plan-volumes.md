---
ms.assetid: 342173ca-4e10-44f4-b2c9-02a6c26f7a4a
title: Planeación de volúmenes en Espacios de almacenamiento directo
ms.prod: windows-server
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 06/28/2019
ms.localizationpriority: medium
ms.openlocfilehash: d5c45b68f18fe3126867a9b6608b0911bb3f63b2
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474762"
---
# <a name="planning-volumes-in-storage-spaces-direct"></a>Planeación de volúmenes en Espacios de almacenamiento directo

> Se aplica a: Windows Server 2019, Windows Server 2016

En este tema se proporcionan instrucciones sobre cómo planear volúmenes en Espacios de almacenamiento directo para satisfacer las necesidades de rendimiento y capacidad de las cargas de trabajo, incluida la elección del sistema de archivos, el tipo de resistencia y el tamaño.

## <a name="review-what-are-volumes"></a>Revisión: ¿Qué son los volúmenes?

Los volúmenes son donde se colocan los archivos que las cargas de trabajo necesitan, como archivos VHD o VHDX para máquinas virtuales de Hyper-V. Los volúmenes combinan las unidades del bloque de almacenamiento para introducir las ventajas de tolerancia a errores, escalabilidad y rendimiento de Espacios de almacenamiento directo.

   >[!NOTE]
   > En toda la documentación de Espacios de almacenamiento directo, usamos el término "volumen" para hacer referencia conjuntamente al volumen y al disco virtual en él, incluida la funcionalidad proporcionada por otras características integradas de Windows, como los volúmenes compartidos de clúster (CSV) y ReFS. Entender estas distinciones en el nivel de implementación no es necesario para planear e implementar Espacios de almacenamiento directo correctamente.

![What-is-Volumes](media/plan-volumes/what-are-volumes.png)

Todos los volúmenes son accesibles para todos los servidores del clúster al mismo tiempo. Una vez creadas, se muestran en **C:\ClusterStorage \\ ** en todos los servidores.

![captura de pantalla de CSV-carpeta](media/plan-volumes/csv-folder-screenshot.png)

## <a name="choosing-how-many-volumes-to-create"></a>Elección del número de volúmenes que se van a crear

Se recomienda que el número de volúmenes sea un múltiplo del número de servidores del clúster. Por ejemplo, si tiene 4 servidores, experimentará un rendimiento más coherente con 4 volúmenes totales que con 3 o 5. Esto permite al clúster distribuir la "propiedad" del volumen (un servidor controla la orquestación de los metadatos de cada volumen) uniformemente entre los servidores.

Se recomienda limitar el número total de volúmenes a:

| Windows Server 2016          | Windows Server 2019          |
|------------------------------|------------------------------|
| Hasta 32 volúmenes por clúster | Hasta 64 volúmenes por clúster |

## <a name="choosing-the-filesystem"></a>Elección del sistema de archivos

Se recomienda usar el nuevo [sistema de archivos resistente (ReFS)](../refs/refs-overview.md) para espacios de almacenamiento directo. ReFS es el propósito principal del sistema de archivos creado para la virtualización y ofrece muchas ventajas, entre las que se incluyen aceleraciones de rendimiento significativas y protección integrada frente a daños en los datos. Admite casi todas las características de NTFS clave, incluida la desduplicación de datos en Windows Server, versión 1709 y posteriores. Vea la [tabla de comparación de características](../refs/refs-overview.md#feature-comparison) de ReFS para obtener más información.

Si la carga de trabajo requiere una característica que ReFS no admite todavía, puede usar NTFS en su lugar.

   > [!TIP]
   > Los volúmenes con distintos sistemas de archivos pueden coexistir en el mismo clúster.

## <a name="choosing-the-resiliency-type"></a>Elección del tipo de resistencia

Los volúmenes de Espacios de almacenamiento directo ofrecen resistencia para protegerse frente a problemas de hardware, como errores de unidad o de servidor, así como para permitir la disponibilidad continua a lo largo del mantenimiento del servidor, como las actualizaciones de software.

   > [!NOTE]
   > Los tipos de resistencia que puede elegir son independientes de los tipos de unidades que tenga.

### <a name="with-two-servers"></a>Con dos servidores

Con dos servidores en el clúster, puede usar la creación de reflejo bidireccional. Si está ejecutando Windows Server 2019, también puede usar la resistencia anidada.

La creación de reflejo bidireccional mantiene dos copias de todos los datos, una copia en las unidades de cada servidor. Su eficiencia de almacenamiento es del 50%; para escribir 1 TB de datos, necesita al menos 2 TB de capacidad de almacenamiento físico en el bloque de almacenamiento. La creación de reflejo bidireccional puede tolerar de manera segura un error de hardware a la vez (un servidor o una unidad).

![two-way-mirror](media/plan-volumes/two-way-mirror.png)

La resistencia anidada (disponible solo en Windows Server 2019) proporciona resistencia de datos entre servidores con creación de reflejo bidireccional y, a continuación, agrega resistencia dentro de un servidor con una paridad de reflejo doble o con aceleración de reflejo. El anidamiento proporciona resistencia de datos incluso cuando un servidor se está reiniciando o no está disponible. Su eficacia de almacenamiento es del 25% con la creación de reflejo bidireccional anidada y de alrededor del 35-40% para la paridad anidada con aceleración de reflejo. La resistencia anidada puede tolerar de manera segura dos errores de hardware a la vez (dos unidades, o un servidor y una unidad en el servidor restante). Debido a esta resistencia de datos agregada, se recomienda usar la resistencia anidada en las implementaciones de producción de clústeres de dos servidores, si ejecuta Windows Server 2019. Para obtener más información, consulte [resistencia anidada](nested-resiliency.md).

![Paridad anidada-con aceleración de reflejo](media/nested-resiliency/nested-mirror-accelerated-parity.png)

### <a name="with-three-servers"></a>Con tres servidores

Con tres servidores, debe usar la creación de reflejo triple para mejorar la tolerancia a errores y el rendimiento. La creación de reflejo triple mantiene tres copias de todos los datos, una copia en las unidades de cada servidor. Su eficiencia de almacenamiento es del 33,3%; para escribir 1 TB de datos, necesita al menos 3 TB de capacidad de almacenamiento físico en el bloque de almacenamiento. La creación de reflejo triple puede tolerar de manera segura [al menos dos problemas de hardware (unidad o servidor) a la vez](storage-spaces-fault-tolerance.md#examples). Si dos nodos dejan de estar disponibles, el grupo de almacenamiento perderá el cuórum, ya que 2/3 de los discos no están disponibles y no se podrá acceder a los discos virtuales. Sin embargo, un nodo puede estar inactivo y se puede producir un error en uno o varios discos de otro nodo y los discos virtuales permanecerán en línea. Por ejemplo, si se está reiniciando un servidor cuando, de repente, se produce un error en otra unidad u otro servidor, la seguridad y la accesibilidad de los datos se mantienen.

![reflejo triple](media/plan-volumes/three-way-mirror.png)

### <a name="with-four-or-more-servers"></a>Con cuatro o más servidores

Con cuatro o más servidores, puede elegir para cada volumen si desea utilizar la creación de reflejo triple, la paridad dual (denominada a menudo "codificación de borrado") o mezclar las dos con la paridad acelerada de reflejo.

La paridad dual proporciona la misma tolerancia a errores que la creación de reflejo triple, pero con una mayor eficacia de almacenamiento. Con cuatro servidores, su eficacia de almacenamiento es 50,0%: para almacenar 2 TB de datos, necesita 4 TB de capacidad de almacenamiento físico en el bloque de almacenamiento. Esto aumenta hasta el 66,7% de la eficiencia del almacenamiento con siete servidores y continúa hasta el 80,0% de eficiencia del almacenamiento. La contrapartida es que la codificación de paridad es más intensiva de proceso, lo que puede limitar su rendimiento.

![paridad doble](media/plan-volumes/dual-parity.png)

El tipo de resistencia que se va a usar depende de las necesidades de la carga de trabajo. Esta es una tabla que resume qué cargas de trabajo son una buena opción para cada tipo de resistencia, así como el rendimiento y la eficiencia de almacenamiento de cada tipo de resistencia.

| Tipo de resistencia | Eficiencia de la capacidad | Velocidad | Cargas de trabajo |
| ------------------- | ----------------------  | --------- | ------------- |
| **Reflejo**         | ![Eficiencia del almacenamiento mostrando un 33%](media/plan-volumes/3-way-mirror-storage-efficiency.png)<br>Reflejo triple: 33% <br>Reflejo bidireccional: 50%     |![Rendimiento que muestra el 100%](media/plan-volumes/three-way-mirror-perf.png)<br> Mayor rendimiento  | Cargas de trabajo virtualizadas<br> Bases de datos<br>Otras cargas de trabajo de alto rendimiento |
| **Paridad acelerada por reflejos** |![Eficiencia del almacenamiento que se muestra alrededor del 50%](media/plan-volumes/mirror-accelerated-parity-storage-efficiency.png)<br> Depende de la proporción de reflejo y paridad | ![Rendimiento en torno al 20%](media/plan-volumes/mirror-accelerated-parity-perf.png)<br>Es mucho más lento que el reflejo, pero hasta dos veces más rápido que la paridad dual.<br> Mejor para lecturas y escrituras secuenciales grandes | Archivado y copia de seguridad<br> Infraestructura de escritorio virtualizado     |
| **Paridad dual**               | ![Eficiencia del almacenamiento que se muestra alrededor del 80%](media/plan-volumes/dual-parity-storage-efficiency.png)<br>4 servidores: 50% <br>16 servidores: hasta 80% | ![Rendimiento que se muestra en torno al 10%](media/plan-volumes/dual-parity-perf.png)<br>Mayor latencia de e/s & uso de CPU en las escrituras<br> Mejor para lecturas y escrituras secuenciales grandes | Archivado y copia de seguridad<br> Infraestructura de escritorio virtualizado  |

#### <a name="when-performance-matters-most"></a>Cuando lo más importante es el rendimiento

Las cargas de trabajo con requisitos de latencia estrictos o que necesitan una gran cantidad de IOPS aleatorias mixtas, como bases de datos de SQL Server o máquinas virtuales de Hyper-V sensibles al rendimiento, deben ejecutarse en volúmenes que usen la creación de reflejo para maximizar el rendimiento.

   >[!TIP]
   > Los reflejos son más rápidos que cualquier otro tipo de resistencia. Usamos la creación de reflejo para casi todos nuestros ejemplos de rendimiento.

#### <a name="when-capacity-matters-most"></a>Cuando la capacidad es más importante

Las cargas de trabajo que escriben con poca frecuencia, como los almacenes de datos o el almacenamiento "en frío", deben ejecutarse en volúmenes que usen paridad dual para maximizar la eficacia del almacenamiento. Algunas otras cargas de trabajo, como los servidores de archivos tradicionales, la infraestructura de escritorio virtual (VDI) u otras que no crean mucho tráfico de e/s aleatoria de desplazamiento rápido o que no requieren el mejor rendimiento también pueden usar la paridad dual, a su discreción. La paridad aumenta inevitablemente el uso de la CPU y la latencia de e/s, especialmente en las escrituras, en comparación con la creación de reflejo.

#### <a name="when-data-is-written-in-bulk"></a>Cuando los datos se escriben de forma masiva

Las cargas de trabajo que escriben en pasadas grandes y secuenciales, como los destinos de archivo o de copia de seguridad, tienen otra opción nueva en Windows Server 2016: un volumen puede mezclar la creación de reflejo y la paridad dual. Escribe las tierras primero en la parte reflejada y se mueven gradualmente a la parte de la paridad más adelante. Esto acelera la ingesta y reduce el uso de recursos cuando llegan Escrituras grandes, lo que permite que se produzca la codificación de paridad de proceso intensivo durante un tiempo más largo. Al cambiar el tamaño de las partes, tenga en cuenta que la cantidad de escrituras que se producen a la vez (por ejemplo, una copia de seguridad diaria) debe ajustarse de forma cómoda en la parte reflejada. Por ejemplo, si ingeri 100 GB una vez al día, considere la posibilidad de usar la creación de reflejo de 150 GB a 200 GB y la paridad dual para el resto.

La eficacia del almacenamiento resultante depende de las proporciones que elija. Vea [esta demostración](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=36m55s) para ver algunos ejemplos.

   > [!TIP]
   > Si observa una disminución brusca en el rendimiento de escritura MSRC durante a través de la ingesta de datos, puede indicar que la parte reflejada no es suficientemente grande o que la paridad acelerada para el reflejo no es adecuada para su caso de uso. Por ejemplo, si el rendimiento de escritura se reduce de 400 MB/s a 40 MB/s, considere la posibilidad de expandir la parte reflejada o cambiar a reflejo triple.

### <a name="about-deployments-with-nvme-ssd-and-hdd"></a>Acerca de las implementaciones con NVMe, SSD y HDD

En implementaciones con dos tipos de unidades, las unidades más rápidas proporcionan almacenamiento en caché mientras que las unidades más lentas proporcionan capacidad. Esto sucede automáticamente: para obtener más información, vea [Descripción de la memoria caché en espacios de almacenamiento directo](understand-the-cache.md). En estas implementaciones, todos los volúmenes residen en última instancia en el mismo tipo de unidades: las unidades de capacidad.

En implementaciones con los tres tipos de unidades, solo las unidades más rápidas (NVMe) proporcionan almacenamiento en caché, con lo que se dejan dos tipos de unidades (SSD y HDD) para proporcionar capacidad. Para cada volumen, puede elegir si reside completamente en la capa de SSD, en su totalidad en el nivel de HDD o si abarca los dos.

   > [!IMPORTANT]
   > Se recomienda usar la capa de SSD para colocar las cargas de trabajo más sensibles al rendimiento en All-Flash.

## <a name="choosing-the-size-of-volumes"></a>Elección del tamaño de los volúmenes

Se recomienda limitar el tamaño de cada volumen a:

| Windows Server 2016 | Windows Server 2019 |
| ------------------- | ------------------- |
| Hasta 32 TB         | Hasta 64 TB         |

   > [!TIP]
   > Si usa una solución de copia de seguridad que se basa en el servicio de instantáneas de volumen (VSS) y el proveedor de software volsnap, como es habitual con las cargas de trabajo de servidor de archivos, si se limita el tamaño del volumen a 10 TB, mejorará el rendimiento y la confiabilidad. Las soluciones de copia de seguridad que usan la versión más reciente de la API RCT de Hyper-V y/o la clonación de bloques ReFS y/o las API nativas de copia de seguridad de SQL funcionan hasta 32 TB y más.

### <a name="footprint"></a>Rastro

El tamaño de un volumen hace referencia a su capacidad utilizable, la cantidad de datos que puede almacenar. Lo proporciona el parámetro **-size** del cmdlet **New-Volume** y, a continuación, aparece en la propiedad **size** al ejecutar el cmdlet **Get-Volume** .

El tamaño es distinto de la *superficie*del volumen, la capacidad total de almacenamiento físico que ocupa en el bloque de almacenamiento. La superficie depende de su tipo de resistencia. Por ejemplo, los volúmenes que usan la creación de reflejo triple tienen una superficie tres veces su tamaño.

Las huellas de los volúmenes deben ajustarse en el bloque de almacenamiento.

![tamaño frente a superficie](media/plan-volumes/size-versus-footprint.png)

### <a name="reserve-capacity"></a>Reservar capacidad

Si se abandona alguna capacidad en el bloque de almacenamiento, se asigna espacio a los volúmenes para reparar "en contexto" después de que se produzca un error en las unidades, lo que mejora la seguridad y el rendimiento de los datos. Si la capacidad es suficiente, una reparación paralela inmediata, en contexto, puede restaurar volúmenes a resistencia completa incluso antes de que se reemplacen las unidades con errores. Esto sucede automáticamente.

Se recomienda reservar el equivalente de una unidad de capacidad por servidor, hasta 4 unidades. Puede reservar más a su discreción, pero esta recomendación mínima garantiza una reparación paralela inmediata y en contexto puede realizarse correctamente después del error de cualquier unidad.

![reserve](media/plan-volumes/reserve.png)

Por ejemplo, si tiene dos servidores y usa unidades de capacidad de 1 TB, Reserve 2 x 1 = 2 TB del grupo como Reserve. Si tiene 3 servidores y unidades de 1 TB de capacidad, Reserve 3 x 1 = 3 TB como reserva. Si tiene 4 o más servidores y unidades de 1 TB de capacidad, Reserve 4 x 1 = 4 TB como reserva.

   >[!NOTE]
   > En clústeres con unidades de los tres tipos (NVMe + SSD + HDD), se recomienda reservar el equivalente de un SSD más un HDD por servidor, hasta 4 unidades de cada uno.

## <a name="example-capacity-planning"></a>Ejemplo: planeamiento de la capacidad

Considere 1 4: clúster de servidores. Cada servidor tiene algunas unidades de caché más de dieciséis unidades de 2 TB de capacidad.

```
4 servers x 16 drives each x 2 TB each = 128 TB
```

Desde este 128 TB en el bloque de almacenamiento, se reservan cuatro unidades u 8 TB, de modo que las reparaciones en contexto pueden producirse sin necesidad de tener prisas de reemplazar unidades después de que se produzcan errores. Esto deja 120 TB de capacidad de almacenamiento físico en el grupo con el que se pueden crear volúmenes.

```
128 TB – (4 x 2 TB) = 120 TB
```

Supongamos que necesitamos nuestra implementación para hospedar algunas máquinas virtuales de Hyper-V altamente activas, pero también tenemos gran cantidad de almacenamiento en frío: archivos y copias de seguridad antiguos que necesitamos conservar. Dado que tenemos cuatro servidores, vamos a crear cuatro volúmenes.

Vamos a colocar las máquinas virtuales en los dos primeros volúmenes, *volume1* y *Volume2*. Elegimos ReFS como sistema de archivos (para la creación y los puntos de comprobación más rápidos) y la creación de reflejo triple para lograr resistencia con el fin de maximizar el rendimiento. Vamos a colocar el almacenamiento en frío en los otros dos volúmenes, el *volumen 3* y el *volumen 4*. Elegimos NTFS como sistema de archivos (para desduplicación de datos) y paridad dual para aumentar la capacidad.

No es necesario que todos los volúmenes tengan el mismo tamaño, pero para simplificar, podemos hacer que sean todos 12 TB.

*Volume1* y *Volume2* ocuparán cada 12 TB x 33,3% de eficiencia = 36 TB de capacidad de almacenamiento físico.

*Volume3* y *Volume4* ocuparán cada 12 TB x 50,0% de eficiencia = 24 TB de capacidad de almacenamiento físico.

```
36 TB + 36 TB + 24 TB + 24 TB = 120 TB
```

Los cuatro volúmenes se ajustan exactamente a la capacidad de almacenamiento físico disponible en nuestro grupo. Pareja!

![ejemplo](media/plan-volumes/example.png)

   >[!TIP]
   > No es necesario crear todos los volúmenes de inmediato. Siempre puede extender volúmenes o crear nuevos volúmenes más adelante.

Para simplificar, en este ejemplo se usan unidades decimales (base 10) en todo, lo que significa que 1 TB = 1 billón bytes. Sin embargo, las cantidades de almacenamiento en Windows aparecen en unidades binarias (base 2). Por ejemplo, cada unidad de 2 TB aparecería como 1,82 TiB en Windows. Del mismo modo, el bloque de almacenamiento de 128 TB aparecería como 116,41 TiB. Se espera que esto sea así.

## <a name="usage"></a>Uso

Vea [crear volúmenes en espacios de almacenamiento directo](create-volumes.md).

### <a name="additional-references"></a>Referencias adicionales

- [Información general de Espacios de almacenamiento directo](storage-spaces-direct-overview.md)
- [Elección de unidades para Espacios de almacenamiento directo](choosing-drives.md)
- [Tolerancia a errores y eficiencia del almacenamiento](storage-spaces-fault-tolerance.md)

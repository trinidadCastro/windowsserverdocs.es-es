---
ms.assetid: 342173ca-4e10-44f4-b2c9-02a6c26f7a4a
title: Planificar volúmenes en Espacios de almacenamiento directo
ms.prod: windows-server
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 06/28/2019
ms.localizationpriority: medium
ms.openlocfilehash: 0825c531913d134cc5711e3c8668fd6dedc4998f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856188"
---
# <a name="planning-volumes-in-storage-spaces-direct"></a>Planificar volúmenes en Espacios de almacenamiento directo

> Se aplica a: Windows Server 2019, Windows Server 2016

En este tema se proporcionan instrucciones para planificar los volúmenes Espacios de almacenamiento directo para satisfacer las necesidades de rendimiento y capacidad de tus cargas de trabajo, incluida la elección de su sistema de archivos, el tipo de resistencia y el tamaño.

## <a name="review-what-are-volumes"></a>Revisión: Qué son los volúmenes

Los volúmenes son donde se colocan los archivos que las cargas de trabajo necesitan, como archivos VHD o VHDX para máquinas virtuales de Hyper-V. Los volúmenes combinan las unidades en el grupo de almacenamiento para introducir las ventajas de tolerancia a errores, escalabilidad y rendimiento de Espacios de almacenamiento directo.

   >[!NOTE]
   > En toda la documentación de Espacios de almacenamiento directo, se usa el término "volumen" para hacer referencia conjuntamente al volumen y al disco virtual contenido, incluida la funcionalidad proporcionada por otras características integradas de Windows, como volúmenes compartidos de clúster (CSV) y ReFS. No es necesario comprender estas diferencias en el nivel de la implementación para planificar e implementar directa Espacios de almacenamiento directo correctamente.

![what-are-volumes](media/plan-volumes/what-are-volumes.png)

Todos los servidores del clúster pueden acceder a todos los volúmenes al mismo tiempo. Una vez creadas, se muestran en **C:\ClusterStorage\\** en todos los servidores.

![csv-folder-screenshot](media/plan-volumes/csv-folder-screenshot.png)

## <a name="choosing-how-many-volumes-to-create"></a>Elegir cuántos volúmenes crear

Te recomendamos que el número de volúmenes sea un múltiplo del número de servidores en el clúster. Por ejemplo, si tiene 4 servidores, experimentará un rendimiento más coherente con 4 volúmenes totales que con 3 o 5. Esto permite al clúster distribuir la "propiedad" de los volúmenes (un servidor controla la organización de metadatos de cada volumen) de forma equitativa entre los servidores.

Se recomienda limitar el número total de volúmenes a:

| Windows Server 2016          | Windows Server 2019          |
|------------------------------|------------------------------|
| Hasta 32 volúmenes por clúster | Hasta 64 volúmenes por clúster |

## <a name="choosing-the-filesystem"></a>Elegir el sistema de archivos

Te recomendamos que uses el nuevo [Sistema de archivos resistente (ReFS)](../refs/refs-overview.md) para Espacios de almacenamiento directo. ReFS es el sistema de archivos principal, diseñado específicamente para la virtualización y ofrece muchas ventajas, como la aceleración drástica del rendimiento y la protección integrada contra los daños en los datos. Admite casi todas las características de NTFS clave, incluida la desduplicación de datos en Windows Server, versión 1709 y posteriores. Vea la [tabla de comparación de características](../refs/refs-overview.md#feature-comparison) de ReFS para obtener más información.

Si la carga de trabajo requiere una característica que ReFS no admite aún, puedes usar NTFS en su lugar.

   > [!TIP]
   > En el mismo clúster pueden coexistir volúmenes con diferentes sistemas de archivos.

## <a name="choosing-the-resiliency-type"></a>Elegir el tipo de resistencia

Los volúmenes en Espacios de almacenamiento directo proporcionan resistencia para proteger frente a problemas de hardware, como errores de servidor o unidades, y para permitir la disponibilidad continua a lo largo del mantenimiento de servidores, como las actualizaciones de software.

   > [!NOTE]
   > Qué tipos de resistencia puedes elegir es independiente de los tipos de unidades que tengas.

### <a name="with-two-servers"></a>Con dos servidores

Con dos servidores en el clúster, puede usar la creación de reflejo bidireccional. Si está ejecutando Windows Server 2019, también puede usar la resistencia anidada.

La creación de reflejo bidireccional mantiene dos copias de todos los datos, una copia en las unidades de cada servidor. Su eficiencia de almacenamiento es del 50%; para escribir 1 TB de datos, necesita al menos 2 TB de capacidad de almacenamiento físico en el bloque de almacenamiento. La creación de reflejo bidireccional puede tolerar de manera segura un error de hardware a la vez (un servidor o una unidad).

![two-way-mirror](media/plan-volumes/two-way-mirror.png)

La resistencia anidada (disponible solo en Windows Server 2019) proporciona resistencia de datos entre servidores con creación de reflejo bidireccional y, a continuación, agrega resistencia dentro de un servidor con una paridad de reflejo doble o con aceleración de reflejo. El anidamiento proporciona resistencia de datos incluso cuando un servidor se está reiniciando o no está disponible. Su eficacia de almacenamiento es del 25% con la creación de reflejo bidireccional anidada y de alrededor del 35-40% para la paridad anidada con aceleración de reflejo. La resistencia anidada puede tolerar de manera segura dos errores de hardware a la vez (dos unidades, o un servidor y una unidad en el servidor restante). Debido a esta resistencia de datos agregada, se recomienda usar la resistencia anidada en las implementaciones de producción de clústeres de dos servidores, si ejecuta Windows Server 2019. Para obtener más información, consulte [resistencia anidada](nested-resiliency.md).

![Paridad anidada-con aceleración de reflejo](media/nested-resiliency/nested-mirror-accelerated-parity.png)

### <a name="with-three-servers"></a>Con tres servidores

Con tres servidores, debes usar la creación de reflejos triple para mejor tolerancia a errores y rendimiento. La creación de reflejos triple mantiene tres copias de todos los datos, una copia en las unidades en cada servidor. Su eficiencia del almacenamiento es del 33,3 %: para escribir 1 TB de datos, se necesitan al menos 3 TB de capacidad de almacenamiento físico en el grupo de almacenamiento. La creación de reflejos triple puede tolerar de forma segura [al menos dos problemas de hardware (en la unidad o el servidor) a la vez](storage-spaces-fault-tolerance.md#examples). Si dos nodos dejan de estar disponibles, el grupo de almacenamiento perderá el cuórum, ya que 2/3 de los discos no están disponibles y no se podrá acceder a los discos virtuales. Sin embargo, un nodo puede estar inactivo y se puede producir un error en uno o varios discos de otro nodo y los discos virtuales permanecerán en línea. Por ejemplo, si estás reiniciando un servidor cuando repentinamente se produce un error en otra unidad o servidor, todos los datos permanecen seguros y accesibles de forma continua.

![reflejo triple](media/plan-volumes/three-way-mirror.png)

### <a name="with-four-or-more-servers"></a>Con cuatro servidores o más

Con cuatro o más servidores, puede elegir para cada volumen si desea utilizar la creación de reflejo triple, la paridad dual (denominada a menudo "codificación de borrado") o mezclar las dos con la paridad acelerada de reflejo.

La paridad dual proporciona la misma tolerancia a errores que la creación de reflejos triple, pero con una mejor eficiencia de almacenamiento. Con cuatro servidores, su eficacia de almacenamiento es 50,0%: para almacenar 2 TB de datos, necesita 4 TB de capacidad de almacenamiento físico en el bloque de almacenamiento. Esto aumenta la eficiencia del almacenamiento a un 66,7 % con siete servidores y continúa hasta una eficiencia de almacenamiento del 80,0 %. El inconveniente es que la codificación de paridad consume más recursos, lo que puede limitar su rendimiento.

![paridad doble](media/plan-volumes/dual-parity.png)

Qué tipo de resistencia debes usar depende de las necesidades de la carga de trabajo. Esta es una tabla que resume qué cargas de trabajo son una buena opción para cada tipo de resistencia, así como el rendimiento y la eficiencia de almacenamiento de cada tipo de resistencia.

| Tipo de resistencia | Eficiencia de la capacidad | Velocidad | Cargas de trabajo |
| ------------------- | ----------------------  | --------- | ------------- |
| **Reflejo**         | ![Eficiencia del almacenamiento mostrando un 33%](media/plan-volumes/3-way-mirror-storage-efficiency.png)<br>Reflejo triple: 33% <br>Reflejo bidireccional: 50%     |![Rendimiento que muestra el 100%](media/plan-volumes/three-way-mirror-perf.png)<br> Mayor rendimiento  | Cargas de trabajo virtualizadas<br> Bases de datos<br>Otras cargas de trabajo de alto rendimiento |
| **Paridad acelerada por reflejos** |![Eficiencia del almacenamiento que se muestra alrededor del 50%](media/plan-volumes/mirror-accelerated-parity-storage-efficiency.png)<br> Depende de la proporción de reflejo y paridad | ![Rendimiento en torno al 20%](media/plan-volumes/mirror-accelerated-parity-perf.png)<br>Es mucho más lento que el reflejo, pero hasta dos veces más rápido que la paridad dual.<br> Mejor para lecturas y escrituras secuenciales grandes | Archivado y copia de seguridad<br> Infraestructura de escritorio virtualizado     |
| **Paridad dual**               | ![Eficiencia del almacenamiento que se muestra alrededor del 80%](media/plan-volumes/dual-parity-storage-efficiency.png)<br>4 servidores: 50% <br>16 servidores: hasta 80% | ![Rendimiento que se muestra en torno al 10%](media/plan-volumes/dual-parity-perf.png)<br>Mayor latencia de e/s & uso de CPU en las escrituras<br> Mejor para lecturas y escrituras secuenciales grandes | Archivado y copia de seguridad<br> Infraestructura de escritorio virtualizado  |

#### <a name="when-performance-matters-most"></a>Cuando lo que más importa es el rendimiento

Las cargas de trabajo que tienen requisitos estrictos de latencia o que necesitan un gran número de IOPS mixtas aleatorias, como bases de datos de SQL Server o máquinas virtuales Hyper-V dependientes del rendimiento, deben ejecutarse en volúmenes que usan la creación de reflejos para maximizar el rendimiento.

   >[!TIP]
   > Los reflejos son más rápidos que cualquier otro tipo de resistencia. Usamos la creación de reflejos para casi todos los ejemplos de rendimiento.

#### <a name="when-capacity-matters-most"></a>Cuando lo que más importa es la capacidad

Las cargas de trabajo que escriben con poca frecuencia, como almacenes de datos o el almacenamiento "frío", se deben ejecutar en volúmenes que usan paridad dual para maximizar la eficiencia del almacenamiento. Ciertas otras cargas de trabajo, como los servidores de archivos tradicionales, la infraestructura de escritorio virtual (VDI) u otras que no crean una gran cantidad de tráfico de E/S aleatorio de desplazamiento rápido o no requieren el mejor rendimiento, pueden usar también la paridad dual, según tu criterio. La paridad inevitablemente aumenta el uso de CPU y la latencia de E/S, en particular al escribir, en comparación con la creación de reflejos.

#### <a name="when-data-is-written-in-bulk"></a>Cuando los datos se escriben en masa

Las cargas de trabajo que escriben en fases grandes y secuenciales, como los destinos de archivado o de copia de seguridad, tienen otra opción que es nueva en Windows Server 2016: un volumen puede combinar creación de reflejos y paridad dual. Las operaciones de escritura se dirigen primero a la porción reflejada y, más adelante, se mueven gradualmente a la porción de paridad. Esto acelera la ingesta y reduce el uso de recursos cuando llegan grandes operaciones de escritura, ya que se permite que la codificación de paridad de consumo intensivo de recursos se produzca a lo largo de un período mayor. Cuando cambies el tamaño de las porciones, ten en cuenta que la cantidad de operaciones de escritura que se producen a la vez (por ejemplo, una copia de seguridad diaria) debe caber con comodidad en la parte reflejada. Por ejemplo, si ingieres 100 GB una vez al día, ten en cuenta usar la creación de reflejos para 150 GB a 200 GB, y la paridad dual para el resto.

La eficiencia del almacenamiento resultante depende de las proporciones que elijas. Consulta [esta demostración](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=36m55s) para ver algunos ejemplos.

   > [!TIP]
   > Si observa una disminución brusca en el rendimiento de escritura MSRC durante a través de la ingesta de datos, puede indicar que la parte reflejada no es suficientemente grande o que la paridad acelerada para el reflejo no es adecuada para su caso de uso. Por ejemplo, si el rendimiento de escritura se reduce de 400 MB/s a 40 MB/s, considere la posibilidad de expandir la parte reflejada o cambiar a reflejo triple.

### <a name="about-deployments-with-nvme-ssd-and-hdd"></a>Acerca de las implementaciones con NVMe, SSD y HDD

En las implementaciones con dos tipos de unidades, las unidades más rápidas proporcionan almacenamiento en caché, mientras que las unidades más lentas proporcionan capacidad. Esto sucede automáticamente: para obtener más información, consulta [Descripción de la memoria caché de Espacios de almacenamiento directo](understand-the-cache.md). En estas implementaciones, todos los volúmenes en definitiva residen en el mismo tipo de unidad: las unidades de capacidad.

En las implementaciones con los tres tipos de unidad, únicamente las unidades más rápidas (NVMe) proporcionan almacenamiento en caché, lo que deja dos tipos de unidad (SSD y HDD) para proporcionar capacidad. Para cada volumen, puedes elegir si reside por completo en la capa SSD, por completo en la capa HDD o si abarca las dos.

   > [!IMPORTANT]
   > Te recomendamos que uses la capa de SSD para colocar las cargas de trabajo más dependientes del rendimiento en todo flash.

## <a name="choosing-the-size-of-volumes"></a>Elegir el tamaño de los volúmenes

Se recomienda limitar el tamaño de cada volumen a:

| Windows Server 2016 | Windows Server 2019 |
| ------------------- | ------------------- |
| Hasta 32 TB         | Hasta 64 TB         |

   > [!TIP]
   > Si usa una solución de copia de seguridad que se basa en el servicio de instantáneas de volumen (VSS) y el proveedor de software volsnap, como es habitual con las cargas de trabajo de servidor de archivos, si se limita el tamaño del volumen a 10 TB, mejorará el rendimiento y la confiabilidad. Las soluciones de copia de seguridad que usan la más reciente API RCT de Hyper-V o clonación de bloques de ReFS o las API nativas de copia de seguridad SQL tienen un buen rendimiento hasta 32 TB y más allá.

### <a name="footprint"></a>Superficie

El tamaño de un volumen se refiere a su capacidad utilizable, la cantidad de datos que puede almacenar. Esto se indica mediante el parámetro **-Size** del cmdlet **New-Volume** y luego aparece en la propiedad **Size** cuando ejecutas el cmdlet **Get-Volume**.

El tamaño es distinto de la *superficie* del volumen, la capacidad de almacenamiento físico total que ocupa en el grupo de almacenamiento. La superficie depende de su tipo de resistencia. Por ejemplo, los volúmenes que usan la creación de reflejos triple tienen una superficie del triple de su tamaño.

Las superficies de los volúmenes deben caber en el grupo de almacenamiento.

![size-versus-footprint](media/plan-volumes/size-versus-footprint.png)

### <a name="reserve-capacity"></a>Capacidad de reserva

Si se deja sin asignar parte de la capacidad en el grupo de almacenamiento, se da espacio a los volúmenes para la reparación "local" después de que se produce un error en las unidades, se mejora el rendimiento y la protección de los datos. Si hay suficiente capacidad, una reparación local inmediata en paralelo puede restaurar los volúmenes a una resistencia completa incluso antes de que se reemplacen las unidades con error. Esto sucede automáticamente.

Te recomendamos reservar el equivalente a una unidad de capacidad por servidor, hasta 4 unidades. A tu criterio, puedes reservar más, pero esta recomendación mínima garantiza que una reparación local inmediata en paralelo se realice correctamente tras el error de cualquiera de las unidades.

![reserve](media/plan-volumes/reserve.png)

Por ejemplo, si tienes 2 servidores y usas unidades de capacidad de 1 TB, aparta 2 x 1 = 2 TB del grupo como reserva. Si tienes 3 servidores y unidades de capacidad de 1 TB, aparta 3 x 1 = 3 TB como reserva. Si tienes 4 servidores o más y unidades de capacidad de 1 TB, aparta 4 x 1 = 4 TB como reserva.

   >[!NOTE]
   > En los clústeres con unidades de los tres tipos (NVMe + SSD + HDD), se recomienda reservar el equivalente a una SSD más una HDD por servidor, hasta 4 unidades de cada una.

## <a name="example-capacity-planning"></a>Ejemplo: Planificación de capacidad

Piensa en un clúster de cuatro servidores. Cada servidor tiene algunas unidades de caché más 16 unidades de 2 TB para capacidad.

```
4 servers x 16 drives each x 2 TB each = 128 TB
```

De estos 128 TB en el grupo de almacenamiento, se apartan cuatro unidades, u 8 TB, para poder realizar las reparaciones locales sin apuro por sustituir las unidades después de que se produce un error. Esto deja 120 TB de capacidad de almacenamiento físico en el grupo con los que se pueden crear volúmenes.

```
128 TB – (4 x 2 TB) = 120 TB
```

Supongamos que la implementación tiene que hospedar algunas máquinas virtuales Hyper-V muy activas, pero también tenemos una gran cantidad de almacenamiento en frío: los archivos antiguos y las copias de seguridad que necesitamos conservar. Ya que hay cuatro servidores, vamos a crear cuatro volúmenes.

Pongamos las máquinas virtuales en los dos primeros volúmenes, *Volume1* y *Volume2*. Elegimos ReFS como sistema de archivos (para creación y puntos de control más rápidos) y la creación de reflejos triple para resistencia a fin de maximizar el rendimiento. Vamos a poner el almacenamiento en frío en los otros dos volúmenes, *Volume 3* y *Volume 4*. Elegimos NTFS como sistema de archivos (para Desduplicación de datos) y paridad dual para resistencia a fin de maximizar la capacidad.

No estamos obligados a que todos los volúmenes tengan el mismo tamaño, pero, por cuestiones de simplicidad, en el ejemplo vamos a hacer que todos sean de 12 TB.

*Volume1* y *Volume2* ocuparán cada uno 12 TB x 33,3 % de eficiencia = 36 TB de capacidad de almacenamiento físico.

*Volume3* y *Volume4* ocuparán cada uno 12 TB x 50,0 % de eficiencia = 24 TB de capacidad de almacenamiento físico.

```
36 TB + 36 TB + 24 TB + 24 TB = 120 TB
```

Los cuatro volúmenes caben exactamente en la capacidad de almacenamiento físico disponible en el grupo. ¡Perfecto!

![ejemplo](media/plan-volumes/example.png)

   >[!TIP]
   > No tienes que crear todos los volúmenes de inmediato. Siempre puedes ampliar los volúmenes o crear nuevos volúmenes más adelante.

Por cuestiones de simplicidad, a lo largo de este ejemplo se usan decimales (base 10), lo que significa que 1 TB = 1 000 000 000 000 bytes. Sin embargo, las cantidades de almacenamiento en Windows aparecen en unidades binarias (base 2). Por ejemplo, cada unidad de 2 TB aparecería como 1,82 TiB en Windows. De igual modo, el grupo de almacenamiento de 128 TB aparecería como 116,41 TiB. Esto es de esperar.

## <a name="usage"></a>Uso

Consulta [Crear volúmenes en Espacios de almacenamiento directo](create-volumes.md).

### <a name="see-also"></a>Vea también

- [Información general de Espacios de almacenamiento directo](storage-spaces-direct-overview.md)
- [Elección de unidades para Espacios de almacenamiento directo](choosing-drives.md)
- [Tolerancia a errores y eficiencia del almacenamiento](storage-spaces-fault-tolerance.md)

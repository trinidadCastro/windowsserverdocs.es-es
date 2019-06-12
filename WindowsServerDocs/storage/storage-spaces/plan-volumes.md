---
ms.assetid: 342173ca-4e10-44f4-b2c9-02a6c26f7a4a
title: Planificar volúmenes en Espacios de almacenamiento directo
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 01/10/2019
ms.localizationpriority: medium
ms.openlocfilehash: c68444be5662480293cee630970d5eb76b52268a
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453190"
---
# <a name="planning-volumes-in-storage-spaces-direct"></a>Planificar volúmenes en Espacios de almacenamiento directo

> Se aplica a: Windows Server 2019, Windows Server 2016

En este tema se proporcionan instrucciones para planificar los volúmenes Espacios de almacenamiento directo para satisfacer las necesidades de rendimiento y capacidad de tus cargas de trabajo, incluida la elección de su sistema de archivos, el tipo de resistencia y el tamaño.

## <a name="review-what-are-volumes"></a>Revisión: ¿Cuáles son los volúmenes

Los volúmenes son los almacenes de datos donde pones los archivos que necesitan tus cargas de trabajo, como los archivos VHD o VHDX para máquinas virtuales Hyper-V. Los volúmenes combinan las unidades en el grupo de almacenamiento para introducir las ventajas de tolerancia a errores, escalabilidad y rendimiento de Espacios de almacenamiento directo.

   >[!NOTE]
   > En toda la documentación de Espacios de almacenamiento directo, se usa el término "volumen" para hacer referencia conjuntamente al volumen y al disco virtual contenido, incluida la funcionalidad proporcionada por otras características integradas de Windows, como volúmenes compartidos de clúster (CSV) y ReFS. No es necesario comprender estas diferencias en el nivel de la implementación para planificar e implementar directa Espacios de almacenamiento directo correctamente.

![what-are-volumes](media/plan-volumes/what-are-volumes.png)

Todos los servidores del clúster pueden acceder a todos los volúmenes al mismo tiempo. Una vez creado, mostrarán en **C:\ClusterStorage\\**  en todos los servidores.

![csv-folder-screenshot](media/plan-volumes/csv-folder-screenshot.png)

## <a name="choosing-how-many-volumes-to-create"></a>Elegir cuántos volúmenes crear

Te recomendamos que el número de volúmenes sea un múltiplo del número de servidores en el clúster. Por ejemplo, si tiene 4 servidores, experimentarán un rendimiento más coherente y 4 volúmenes total que con 3 o 5. Esto permite al clúster distribuir la "propiedad" de los volúmenes (un servidor controla la organización de metadatos de cada volumen) de forma equitativa entre los servidores.

Le recomendamos que limite el número total de los volúmenes:

| Windows Server 2016          | Windows Server 2019          |
|------------------------------|------------------------------|
| Hasta 32 volúmenes por clúster | Volúmenes de hasta 64 por clúster |

## <a name="choosing-the-filesystem"></a>Elegir el sistema de archivos

Te recomendamos que uses el nuevo [Sistema de archivos resistente (ReFS)](../refs/refs-overview.md) para Espacios de almacenamiento directo. ReFS es el sistema de archivos principal, diseñado específicamente para la virtualización y ofrece muchas ventajas, como la aceleración drástica del rendimiento y la protección integrada contra los daños en los datos. Admite casi todas las características clave de NTFS, incluidas la desduplicación de datos en Windows Server, versión 1709 y versiones posterior. Consulte las referencias [tabla comparativa de características](../refs/refs-overview.md#feature-comparison) para obtener más información.

Si la carga de trabajo requiere una característica que ReFS no admite aún, puedes usar NTFS en su lugar.

   >[!TIP]
   > En el mismo clúster pueden coexistir volúmenes con diferentes sistemas de archivos.

## <a name="choosing-the-resiliency-type"></a>Elegir el tipo de resistencia

Los volúmenes en Espacios de almacenamiento directo proporcionan resistencia para proteger frente a problemas de hardware, como errores de servidor o unidades, y para permitir la disponibilidad continua a lo largo del mantenimiento de servidores, como las actualizaciones de software.

   >[!NOTE]
   > Qué tipos de resistencia puedes elegir es independiente de los tipos de unidades que tengas.

### <a name="with-two-servers"></a>Con dos servidores

La única opción para clústeres con dos servidores es la creación de reflejos dobles. Esto mantiene dos copias de todos los datos, una copia en las unidades en cada servidor. Su eficiencia del almacenamiento es del 50 %: para escribir 1 TB de datos, se necesitan al menos 2 TB de capacidad de almacenamiento físico en el grupo de almacenamiento. La creación de reflejos dobles puede tolerar con seguridad un error de hardware (en la unidad o el servidor) a la vez.

![two-way-mirror](media/plan-volumes/two-way-mirror.png)

Si tienes más de dos servidores, te recomendamos usar uno de los siguientes tipos de resistencia en su lugar.

### <a name="with-three-servers"></a>Con tres servidores

Con tres servidores, debes usar la creación de reflejos triple para mejor tolerancia a errores y rendimiento. La creación de reflejos triple mantiene tres copias de todos los datos, una copia en las unidades en cada servidor. Su eficiencia del almacenamiento es del 33,3 %: para escribir 1 TB de datos, se necesitan al menos 3 TB de capacidad de almacenamiento físico en el grupo de almacenamiento. La creación de reflejos triple puede tolerar de forma segura [al menos dos problemas de hardware (en la unidad o el servidor) a la vez](storage-spaces-fault-tolerance.md#examples). Si 2 nodos dejen de estar disponibles el bloque de almacenamiento perderá quórum, ya que 2/3 de los discos no están disponibles y los discos virtuales estarán inaccesibles. Sin embargo, puede ser un nodo hacia abajo y pueden producir un error en uno o más discos en otro nodo y los discos virtuales seguirán estando en línea. Por ejemplo, si estás reiniciando un servidor cuando repentinamente se produce un error en otra unidad o servidor, todos los datos permanecen seguros y accesibles de forma continua.

![reflejo triple](media/plan-volumes/three-way-mirror.png)

### <a name="with-four-or-more-servers"></a>Con cuatro servidores o más

Con cuatro servidores o más, puedes elegir para cada volumen si vas a usar la creación de reflejos triple, paridad dual (a menudo llamada "codificación de borrado") o una combinación de las dos.

La paridad dual proporciona la misma tolerancia a errores que la creación de reflejos triple, pero con una mejor eficiencia de almacenamiento. Con cuatro servidores, su eficiencia del almacenamiento es del 50,0 %: para almacenar 2 TB de datos, se necesitan 4 TB de capacidad de almacenamiento físico en el grupo de almacenamiento. Esto aumenta la eficiencia del almacenamiento a un 66,7 % con siete servidores y continúa hasta una eficiencia de almacenamiento del 80,0 %. El inconveniente es que la codificación de paridad consume más recursos, lo que puede limitar su rendimiento.

![paridad doble](media/plan-volumes/dual-parity.png)

Qué tipo de resistencia debes usar depende de las necesidades de la carga de trabajo. Esta es una tabla que resume qué cargas de trabajo son una buena opción para cada tipo de resistencia, así como la eficacia de almacenamiento y rendimiento de cada tipo de resistencia.

| **Tipo de resistencia**| **Eficacia de capacidad**| **Velocidad**| **Cargas de trabajo**
|--------------------|--------------------------------|--------------------------------|--------------------------
| **Reflejo**         | ![Que muestra la eficacia de almacenamiento 33%](media/plan-volumes/3-way-mirror-storage-efficiency.png)<br>Triple: 33% <br>Dos de forma reflejo: 50 %     |![Mostrando rendimiento 100%](media/plan-volumes/three-way-mirror-perf.png)<br> Mayor rendimiento  | Cargas de trabajo virtualizadas<br> Bases de datos<br>Otras cargas de trabajo de alto rendimiento |
| **Paridad acelerada por reflejos** |![Eficacia de almacenamiento que muestra aproximadamente 50%](media/plan-volumes/mirror-accelerated-parity-storage-efficiency.png)<br> Depende de proporción de reflejo y paridad | ![Que se muestra aproximadamente el 20% de rendimiento](media/plan-volumes/mirror-accelerated-parity-perf.png)<br>Mucho más lento que reflejan, pero hasta dos veces tan rápido como paridad dual<br> Lo mejor para lecturas y escrituras secuenciales grandes | Archivado y copia de seguridad<br> Infraestructura de escritorio virtualizado     |
| **Paridad dual**               | ![Eficacia de almacenamiento que se muestra aproximadamente el 80%](media/plan-volumes/dual-parity-storage-efficiency.png)<br>4 servidores: 50 % <br>16 servidores: hasta un 80% | ![Rendimiento que muestra aproximadamente 10%](media/plan-volumes/dual-parity-perf.png)<br>Uso de CPU en las escrituras & mayor latencia de E/S<br> Lo mejor para lecturas y escrituras secuenciales grandes | Archivado y copia de seguridad<br> Infraestructura de escritorio virtualizado  |

#### <a name="when-performance-matters-most"></a>Cuando lo que más importa es el rendimiento

Las cargas de trabajo que tienen requisitos estrictos de latencia o que necesitan un gran número de IOPS mixtas aleatorias, como bases de datos de SQL Server o máquinas virtuales Hyper-V dependientes del rendimiento, deben ejecutarse en volúmenes que usan la creación de reflejos para maximizar el rendimiento.

   >[!TIP]
   > Los reflejos son más rápidos que cualquier otro tipo de resistencia. Usamos la creación de reflejos para casi todos los ejemplos de rendimiento.

#### <a name="when-capacity-matters-most"></a>Cuando lo que más importa es la capacidad

Las cargas de trabajo que escriben con poca frecuencia, como almacenes de datos o el almacenamiento "frío", se deben ejecutar en volúmenes que usan paridad dual para maximizar la eficiencia del almacenamiento. Ciertas otras cargas de trabajo, como los servidores de archivos tradicionales, la infraestructura de escritorio virtual (VDI) u otras que no crean una gran cantidad de tráfico de E/S aleatorio de desplazamiento rápido o no requieren el mejor rendimiento, pueden usar también la paridad dual, según tu criterio. La paridad inevitablemente aumenta el uso de CPU y la latencia de E/S, en particular al escribir, en comparación con la creación de reflejos.

#### <a name="when-data-is-written-in-bulk"></a>Cuando los datos se escriben en masa

Las cargas de trabajo que escriben en fases grandes y secuenciales, como los destinos de archivado o de copia de seguridad, tienen otra opción que es nueva en Windows Server 2016: un volumen puede combinar creación de reflejos y paridad dual. Las operaciones de escritura se dirigen primero a la porción reflejada y, más adelante, se mueven gradualmente a la porción de paridad. Esto acelera la ingesta y reduce el uso de recursos cuando llegan grandes operaciones de escritura, ya que se permite que la codificación de paridad de consumo intensivo de recursos se produzca a lo largo de un período mayor. Cuando cambies el tamaño de las porciones, ten en cuenta que la cantidad de operaciones de escritura que se producen a la vez (por ejemplo, una copia de seguridad diaria) debe caber con comodidad en la parte reflejada. Por ejemplo, si ingieres 100 GB una vez al día, ten en cuenta usar la creación de reflejos para 150 GB a 200 GB, y la paridad dual para el resto.

La eficiencia del almacenamiento resultante depende de las proporciones que elijas. Consulta [esta demostración](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=36m55s) para ver algunos ejemplos.

   >[!TIP]
   > Si observa una disminución repentina en el rendimiento de escritura mitad a través de injestion de datos, puede indicar que la parte de la réplica no es suficientemente grande o que acelerada reflejado paridad no es adecuado para su caso de uso. Por ejemplo, si escribir disminuciones de rendimiento de 400 MB/s a 40 MB/s, considere la posibilidad de expandir la parte reflejo o cambiar a triple.

### <a name="about-deployments-with-nvme-ssd-and-hdd"></a>Acerca de las implementaciones con NVMe, SSD y HDD

En las implementaciones con dos tipos de unidades, las unidades más rápidas proporcionan almacenamiento en caché, mientras que las unidades más lentas proporcionan capacidad. Esto sucede automáticamente: para obtener más información, consulta [Descripción de la memoria caché de Espacios de almacenamiento directo](understand-the-cache.md). En estas implementaciones, todos los volúmenes en definitiva residen en el mismo tipo de unidad: las unidades de capacidad.

En las implementaciones con los tres tipos de unidad, únicamente las unidades más rápidas (NVMe) proporcionan almacenamiento en caché, lo que deja dos tipos de unidad (SSD y HDD) para proporcionar capacidad. Para cada volumen, puedes elegir si reside por completo en la capa SSD, por completo en la capa HDD o si abarca las dos.

   >[!IMPORTANT]
   > Te recomendamos que uses la capa de SSD para colocar las cargas de trabajo más dependientes del rendimiento en todo flash.

## <a name="choosing-the-size-of-volumes"></a>Elegir el tamaño de los volúmenes

Le recomendamos que limite el tamaño de cada volumen para:

| Windows Server 2016 | Windows Server 2019 |
|---------------------|---------------------|
| Hasta 32 TB         | Hasta 64 TB         |

   >[!TIP]
   > Si usas una solución de copia de seguridad que se basa en el Servicio de instantáneas de volumen (VSS) y el proveedor de software Volsnap —como es común con las cargas de trabajo de servidores de archivos—, limitar el tamaño de los volúmenes a 10 TB mejorará el rendimiento y la confiabilidad. Las soluciones de copia de seguridad que usan la más reciente API RCT de Hyper-V o clonación de bloques de ReFS o las API nativas de copia de seguridad SQL tienen un buen rendimiento hasta 32 TB y más allá.

### <a name="footprint"></a>Superficie

El tamaño de un volumen se refiere a su capacidad utilizable, la cantidad de datos que puede almacenar. Esto se indica mediante el parámetro **-Size** del cmdlet **New-Volume** y luego aparece en la propiedad **Size** cuando ejecutas el cmdlet **Get-Volume**.

El tamaño es distinto de la *superficie* del volumen, la capacidad de almacenamiento físico total que ocupa en el grupo de almacenamiento. La superficie depende de su tipo de resistencia. Por ejemplo, los volúmenes que usan la creación de reflejos triple tienen una superficie del triple de su tamaño.

Las superficies de los volúmenes deben caber en el grupo de almacenamiento.

![size-versus-footprint](media/plan-volumes/size-versus-footprint.png)

### <a name="reserve-capacity"></a>Capacidad de reserva

Si se deja sin asignar parte de la capacidad en el grupo de almacenamiento, se da espacio a los volúmenes para la reparación "local" después de que se produce un error en las unidades, se mejora el rendimiento y la protección de los datos. Si hay suficiente capacidad, una reparación local inmediata en paralelo puede restaurar los volúmenes a una resistencia completa incluso antes de que se reemplacen las unidades con error. Esto sucede automáticamente.

Te recomendamos reservar el equivalente a una unidad de capacidad por servidor, hasta 4 unidades. A tu criterio, puedes reservar más, pero esta recomendación mínima garantiza que una reparación local inmediata en paralelo se realice correctamente tras el error de cualquiera de las unidades.

![reserva](media/plan-volumes/reserve.png)

Por ejemplo, si tienes 2 servidores y usas unidades de capacidad de 1 TB, aparta 2 x 1 = 2 TB del grupo como reserva. Si tienes 3 servidores y unidades de capacidad de 1 TB, aparta 3 x 1 = 3 TB como reserva. Si tienes 4 servidores o más y unidades de capacidad de 1 TB, aparta 4 x 1 = 4 TB como reserva.

   >[!NOTE]
   > En los clústeres con unidades de los tres tipos (NVMe + SSD + HDD), se recomienda reservar el equivalente a una SSD más una HDD por servidor, hasta 4 unidades de cada una.

## <a name="example-capacity-planning"></a>Por ejemplo: Planificación de capacidad

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

- [Información general de espacios directo de almacenamiento](storage-spaces-direct-overview.md)
- [Elección de las unidades para espacios de almacenamiento directo](choosing-drives.md)
- [Tolerancia a errores y eficiencia del almacenamiento](storage-spaces-fault-tolerance.md)

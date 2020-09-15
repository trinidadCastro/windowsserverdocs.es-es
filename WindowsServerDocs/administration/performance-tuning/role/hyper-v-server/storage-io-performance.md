---
title: Rendimiento de e/s de almacenamiento de Hyper-V
description: Consideraciones de rendimiento de e/s de almacenamiento en el ajuste del rendimiento de Hyper-V
ms.topic: article
ms.author: asmahi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 762ff719eb60a2fbcec61c0b9b6cb2e14f9ba677
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078223"
---
# <a name="hyper-v-storage-io-performance"></a>Rendimiento de e/s de almacenamiento de Hyper-V

En esta sección se describen las distintas opciones y consideraciones para optimizar el rendimiento de e/s de almacenamiento en una máquina virtual. La ruta de acceso de e/s de almacenamiento se extiende desde la pila de almacenamiento de invitado, a través de la capa de virtualización del host, a la pila de almacenamiento del host y, a continuación, al disco físico. A continuación se muestran explicaciones sobre cómo las optimizaciones son posibles en cada una de estas fases.

## <a name="virtual-controllers"></a>Controladores virtuales

Hyper-V ofrece tres tipos de controladores virtuales: IDE, SCSI y adaptadores de bus host (HBA) virtuales.

## <a name="ide"></a>IDE

Los controladores IDE exponen los discos IDE a la máquina virtual. La controladora IDE está emulada y es el único controlador que está disponible para las máquinas virtuales invitadas que ejecutan una versión anterior de Windows sin la máquina virtual Integration Services. La e/s de disco que se realiza mediante el controlador de filtro IDE que se proporciona con la máquina virtual Integration Services es mucho mejor que el rendimiento de e/s de disco que se proporciona con el controlador IDE emulado. Se recomienda usar discos IDE solo para los discos del sistema operativo, ya que tienen limitaciones de rendimiento debido al tamaño máximo de e/s que se puede emitir para estos dispositivos.

## <a name="scsi-sas-controller"></a>SCSI (controlador SAS)

Los controladores SCSI exponen discos SCSI a la máquina virtual, y cada controlador SCSI virtual puede admitir hasta 64 dispositivos. Para obtener un rendimiento óptimo, se recomienda conectar varios discos a una sola controladora SCSI virtual y crear controladores adicionales solo a medida que se requieran para escalar el número de discos conectados a la máquina virtual. La ruta de acceso SCSI no se emula, lo que hace que sea el controlador preferido para cualquier disco que no sea el disco del sistema operativo. De hecho, con las máquinas virtuales de generación 2, es el único tipo de controlador posible. En Windows Server 2012 R2, este controlador se muestra como SAS para admitir VHDX compartido.

## <a name="virtual-fibre-channel-hbas"></a>HBA de Canal de fibra virtual

Los HBA Canal de fibra virtuales se pueden configurar para permitir el acceso directo a las máquinas virtuales a Canal de fibra y Canal de fibra a través de LUN Ethernet (FCoE). Los discos Canal de fibra virtuales omiten el sistema de archivos NTFS en la partición raíz, lo que reduce el uso de CPU de la e/s de almacenamiento.

Las unidades de datos de gran tamaño y las unidades que se comparten entre varias máquinas virtuales (para escenarios de clústeres invitados) son candidatas principales para los discos de Canal de fibra virtual.

Los discos Canal de fibra virtuales requieren la instalación de uno o varios adaptadores de bus host (HBA) Canal de fibra en el host. Cada HBA de host es necesario para usar un controlador HBA compatible con las funcionalidades Canal de fibra/NPIV virtuales de Windows Server 2016. El tejido de SAN debe admitir NPIV, y los puertos HBA que se usan para el Canal de fibra virtual deben configurarse en una topología de Canal de fibra que admita NPIV.

Para maximizar el rendimiento en los hosts que se instalan con más de un HBA, se recomienda configurar varios HBAs virtuales dentro de la máquina virtual de Hyper-V (se pueden configurar hasta cuatro HBA para cada máquina virtual). Hyper-V realizará automáticamente un mejor esfuerzo por equilibrar HBAs virtuales para hospedar HBAs que tengan acceso a la misma SAN virtual.

## <a name="virtual-disks"></a>Discos virtuales

Los discos se pueden exponer a las máquinas virtuales a través de las controladoras virtuales. Estos discos podrían ser discos duros virtuales que son abstracciones de archivos de un disco o un disco de acceso directo en el host.

## <a name="virtual-hard-disks"></a>Discos duros virtuales

Hay dos formatos de disco duro virtual, VHD y VHDX. Cada uno de estos formatos admite tres tipos de archivos de disco duro virtual.

## <a name="vhd-format"></a>Formato VHD

El formato VHD era el único formato de disco duro virtual compatible con Hyper-V en versiones anteriores. Introducido en Windows Server 2012, el formato VHD se ha modificado para permitir una mejor alineación, lo que da como resultado un rendimiento significativamente mejor en los nuevos discos de sector grande.

Cualquier nuevo VHD que se cree en Windows Server 2012 o una versión más reciente tendrá una alineación óptima de 4 KB. Este formato alineado es totalmente compatible con los sistemas operativos Windows Server anteriores. Sin embargo, la propiedad Alignment se interrumpirá para las nuevas asignaciones de los analizadores que no son compatibles con la alineación de 4 KB (por ejemplo, un analizador de VHD de una versión anterior de Windows Server o un analizador que no es de Microsoft).

Cualquier VHD que se mueva de una versión anterior no se convierte automáticamente a este nuevo formato de VHD mejorado.

Para convertir al nuevo formato VHD, ejecute el siguiente comando de Windows PowerShell:

``` syntax
Convert-VHD –Path E:\vms\testvhd\test.vhd –DestinationPath E:\vms\testvhd\test-converted.vhd
```

Puede comprobar la propiedad Alignment de todos los VHD del sistema y debe convertirse a la alineación óptima de 4 KB. Puede crear un nuevo VHD con los datos del VHD original mediante la opción **crear desde el origen** .

Para comprobar la alineación mediante Windows PowerShell, examine la línea de alineación, como se muestra a continuación:

``` syntax
Get-VHD –Path E:\vms\testvhd\test.vhd

Path                    : E:\vms\testvhd\test.vhd
VhdFormat               : VHD
VhdType                 : Dynamic
FileSize                : 69245440
Size                    : 10737418240
MinimumSize             : 10735321088
LogicalSectorSize       : 512
PhysicalSectorSize      : 512
BlockSize               : 2097152
ParentPath              :
FragmentationPercentage : 10
Alignment               : 0
Attached                : False
DiskNumber              :
IsDeleted               : False
Number                  :
```

Para comprobar la alineación mediante Windows PowerShell, examine la línea de alineación, como se muestra a continuación:

``` syntax
Get-VHD –Path E:\vms\testvhd\test-converted.vhd

Path                    : E:\vms\testvhd\test-converted.vhd
VhdFormat               : VHD
VhdType                 : Dynamic
FileSize                : 69369856
Size                    : 10737418240
MinimumSize             : 10735321088
LogicalSectorSize       : 512
PhysicalSectorSize      : 512
BlockSize               : 2097152
ParentPath              :
FragmentationPercentage : 0
Alignment               : 1
Attached                : False
DiskNumber              :
IsDeleted               : False
Number                  :
```

## <a name="vhdx-format"></a>Formato VHDX

VHDX es un nuevo formato de disco duro virtual introducido en Windows Server 2012, que permite crear discos virtuales resistentes de alto rendimiento de hasta 64 terabytes. Las ventajas de este formato incluyen:

-   Compatibilidad con la capacidad de almacenamiento de disco duro virtual de hasta 64 terabytes.

-   Protección contra los daños en los datos durante las caídas del suministro eléctrico mediante el registro de las actualizaciones en las estructuras de metadatos de VHDX.

-   Capacidad de almacenar metadatos personalizados sobre un archivo, que un usuario podría querer grabar, como la versión del sistema operativo o las revisiones aplicadas.

El formato VHDX también proporciona las siguientes ventajas de rendimiento:

-   Alineación mejorada del formato de disco duro virtual para que funcione bien en los discos de sector grande.

-   Tamaños de bloque más grandes para discos dinámicos y diferenciales, lo que permite que estos discos se adapten a las necesidades de la carga de trabajo.

-   disco virtual de sector lógico de 4 KB que permite aumentar el rendimiento cuando lo usan las aplicaciones y cargas de trabajo diseñadas para sectores de 4 KB.

-   Eficacia en la representación de datos, lo que da como resultado un tamaño de archivo más pequeño y permite que el dispositivo de almacenamiento físico subyacente reclame el espacio no utilizado. (El recorte requiere discos de acceso directo o SCSI y hardware compatible con el recorte).

Al actualizar a Windows Server 2016, se recomienda convertir todos los archivos VHD al formato VHDX debido a estas ventajas. El único escenario en el que tendría sentido mantener los archivos en el formato VHD es cuando una máquina virtual tiene la posibilidad de moverse a una versión anterior de Hyper-V que no admite el formato VHDX.

## <a name="types-of-virtual-hard-disk-files"></a>Tipos de archivos de disco duro virtual

Hay tres tipos de archivos VHD. En las secciones siguientes se muestran las características de rendimiento y las ventajas e inconvenientes entre los tipos.

Se deben tener en cuenta las siguientes recomendaciones con respecto a la selección de un tipo de archivo VHD:

-   Cuando se usa el formato VHD, se recomienda usar el tipo fijo porque tiene mejores características de resistencia y rendimiento en comparación con los demás tipos de archivo VHD.

-   Cuando se usa el formato VHDX, se recomienda usar el tipo dinámico, ya que ofrece garantías de resistencia además del ahorro de espacio asociado a la asignación de espacio solo cuando es necesario hacerlo.

-   También se recomienda el tipo Fixed, independientemente del formato, cuando el almacenamiento en el volumen de hospedaje no se supervisa de forma activa para asegurarse de que hay suficiente espacio en disco al expandir el archivo VHD en tiempo de ejecución.

-   Las instantáneas de una máquina virtual crean un VHD de diferenciación para almacenar escrituras en los discos. Tener solo unas pocas instantáneas puede elevar el uso de CPU de las operaciones de e/s de almacenamiento, pero puede que no afecte de forma notable al rendimiento excepto en cargas de trabajo de servidor con un gran número de e/s. Sin embargo, tener una gran cadena de instantáneas puede afectar notablemente al rendimiento, ya que la lectura del VHD puede requerir la comprobación de los bloques solicitados en muchos discos duros virtuales de diferenciación. Mantener las cadenas de instantáneas cortas es importante para mantener un buen rendimiento de e/s de disco.

## <a name="fixed-virtual-hard-disk-type"></a>Tipo de disco duro virtual fijo

El espacio del VHD se asigna por primera vez cuando se crea el archivo del disco duro virtual. Es menos probable que este tipo de archivo VHD fragmente, lo que reduce el rendimiento de e/s cuando una sola e/s se divide en varias operaciones de e/s. Tiene la sobrecarga de CPU más baja de los tres tipos de archivo VHD porque las lecturas y escrituras no necesitan buscar la asignación del bloque.

## <a name="dynamic-virtual-hard-disk-type"></a>Tipo de disco duro virtual dinámico

El espacio para el disco duro virtual se asigna a petición. Los bloques del disco se inician como bloques sin asignar y no están respaldados por ningún espacio real del archivo. Cuando se escribe un bloque por primera vez en, la pila de virtualización debe asignar espacio dentro del archivo VHD para el bloque y, a continuación, actualizar los metadatos. Esto aumenta el número de operaciones de e/s de disco necesarias para la escritura y aumenta el uso de la CPU. Las operaciones de lectura y escritura en los bloques existentes incurren en el acceso al disco y la sobrecarga de la CPU al buscar la asignación de bloques en los metadatos.

## <a name="differencing-virtual-hard-disk-type"></a>Tipo de disco duro virtual de diferenciación

El disco duro virtual apunta a un archivo VHD primario. Cualquier escritura en bloques que no se escriban para dar lugar a que se asigne espacio en el archivo VHD, como con un VHD de expansión dinámica. Las lecturas se prestan desde el archivo VHD si se ha escrito en el bloque. De lo contrario, se les presta servicio desde el archivo VHD primario. En ambos casos, los metadatos se leen para determinar la asignación del bloque. Las lecturas y escrituras en este VHD pueden consumir más CPU y producir más e/s que un archivo VHD fijo.

## <a name="block-size-considerations"></a>Consideraciones de tamaño de bloque

El tamaño de bloque puede afectar significativamente al rendimiento. Es óptimo que coincida con el tamaño de bloque con los patrones de asignación de la carga de trabajo que está utilizando el disco. Por ejemplo, si una aplicación se asigna en fragmentos de 16 MB, sería óptimo tener un tamaño de bloque de disco duro virtual de 16 MB. Un tamaño de bloque de &gt; 2 MB solo es posible en discos duros virtuales con el formato VHDX. Tener un tamaño de bloque mayor que el patrón de asignación para una carga de trabajo de e/s aleatoria aumentará significativamente el uso de espacio en el host.

## <a name="sector-size-implications"></a>Implicaciones de tamaño de sector

La mayor parte de la industria del software ha dependido de sectores de disco de 512 bytes, pero el estándar se está trasladando a sectores de disco de 4 KB. Para reducir los problemas de compatibilidad que pueden surgir de un cambio en el tamaño del sector, los proveedores de unidades de disco duro están introduciendo un tamaño transitorio conocido como unidades de emulación 512 (512e).

Estas unidades de emulación ofrecen algunas de las ventajas que ofrecen las unidades nativas del sector del disco de 4 KB, como la eficacia mejorada del formato y un esquema mejorado para códigos de corrección de errores (ECC). Presentan menos problemas de compatibilidad que se producirían al exponer un tamaño de sector de 4 KB en la interfaz de disco.

## <a name="support-for-512e-disks"></a>Compatibilidad con discos 512e

Un disco 512e puede realizar una escritura solo en términos de un sector físico, es decir, no puede escribir directamente un sector 512byte que se le emita. El proceso interno en el disco que realiza estas escrituras puede seguir estos pasos:

-   El disco lee el sector físico de 4 KB en su caché interna, que contiene el sector lógico de 512 bytes al que se hace referencia en la escritura.

-   Los datos en el búfer de 4 KB se modifican para incluir el sector de 512 bytes actualizado.

-   El disco vuelve a realizar una escritura del búfer de 4 KB actualizado en su sector físico del disco.

Este proceso se denomina lectura, modificación y escritura (RMW). El impacto general en el rendimiento del proceso RMW depende de las cargas de trabajo. El proceso RMW provoca la degradación del rendimiento en los discos duros virtuales por los siguientes motivos:

-   Los discos duros virtuales dinámicos y de diferenciación tienen un mapa de bits del sector de 512 bytes delante de su carga de datos. Además, los localizadores de pie de página, encabezado y primario se alinean con un sector de 512 bytes. Es habitual que el controlador de disco duro virtual emita comandos de escritura de 512 bytes para actualizar estas estructuras, lo que produce el proceso RMW descrito anteriormente.

-   Normalmente, las aplicaciones emiten lecturas y escrituras en múltiplos de tamaños de 4 KB (el tamaño de clúster predeterminado de NTFS). Dado que hay un mapa de bits del sector de 512 bytes delante del bloque de carga de datos de los discos duros virtuales dinámicos y de diferenciación, los bloques de 4 KB no se alinean con el límite físico de 4 KB. En la ilustración siguiente se muestra un bloque VHD de 4 KB (resaltado) que no está alineado con el límite físico de 4 KB.

![bloque VHD de 4 KB](../../media/perftune-guide-vhd-4kb-block.png)

Cada comando de escritura de 4 KB que emite el analizador actual para actualizar los resultados de los datos de carga en dos lecturas para dos bloques del disco, que se actualizan y posteriormente se vuelven a escribir en los dos bloques de disco. Hyper-V en Windows Server 2016 mitiga algunos de los efectos de rendimiento en discos 512e en la pila de VHD mediante la preparación de las estructuras mencionadas anteriormente para la alineación de los límites de 4 KB en el formato VHD. Esto evita el efecto RMW al tener acceso a los datos del archivo de disco duro virtual y al actualizar las estructuras de metadatos del disco duro virtual.

Como se mencionó anteriormente, los VHD que se copian de versiones anteriores de Windows Server no se alinearán automáticamente a 4 KB. Puede convertirlos manualmente para alinearlos de forma óptima mediante la opción **Copiar del disco de origen** que está disponible en las interfaces de VHD.

De forma predeterminada, los VHD se exponen con un tamaño de sector físico de 512 bytes. Esto se hace para asegurarse de que las aplicaciones dependientes del tamaño del sector físico no se vean afectadas cuando la aplicación y los discos duros virtuales se mueven de una versión anterior de Windows Server.

De forma predeterminada, los discos con el formato VHDX se crean con el tamaño de sector físico de 4 KB para optimizar sus discos normales de Perfil de rendimiento y discos de sector grande. Para hacer un uso completo de los sectores de 4 KB, se recomienda usar el formato VHDX.

## <a name="support-for-native-4kb-disks"></a>Compatibilidad con discos nativos de 4 KB

Hyper-V en Windows Server 2012 R2 y versiones posteriores admite discos nativos de 4 KB. Pero todavía es posible almacenar el disco VHD en un disco nativo de 4 KB. Esto se hace implementando un algoritmo de RMW de software en la capa de la pila de almacenamiento virtual que convierte el acceso de 512 bytes y las solicitudes de actualización a las actualizaciones y los accesos de 4 KB correspondientes.

Dado que el archivo VHD solo puede exponerse como discos de tamaño de sector lógico de 512 bytes, es muy probable que haya aplicaciones que emitan solicitudes de e/s de 512 bytes. En estos casos, el nivel RMW satisfará estas solicitudes y provocará una degradación del rendimiento. Esto también se aplica a un disco formateado con VHDX que tiene un tamaño de sector lógico de 512 bytes.

Es posible configurar un archivo VHDX para que se exponga como un disco de tamaño de sector lógico de 4 KB, y esto sería una configuración óptima para el rendimiento cuando el disco esté hospedado en un dispositivo físico nativo de 4 KB. Se debe tener cuidado para asegurarse de que el invitado y la aplicación que usa el disco virtual están respaldados por el tamaño de sector lógico de 4 KB. El formato VHDX funcionará correctamente en un dispositivo de tamaño de sector lógico de 4 KB.

## <a name="pass-through-disks"></a>Discos de acceso directo

El VHD de una máquina virtual se puede asignar directamente a un disco físico o a un número de unidad lógica (LUN), en lugar de a un archivo VHD. La ventaja es que esta configuración omite el sistema de archivos NTFS en la partición raíz, lo que reduce el uso de CPU de la e/s de almacenamiento. El riesgo es que los discos físicos o LUN pueden ser más difíciles de cambiar entre máquinas que los archivos VHD.

Los discos de acceso directo deben evitarse debido a las limitaciones introducidas en escenarios de migración de máquinas virtuales.

## <a name="advanced-storage-features"></a>Características de almacenamiento avanzadas

### <a name="storage-quality-of-service-qos"></a>Calidad de servicio (QoS) del almacenamiento

A partir de Windows Server 2012 R2, Hyper-V incluye la capacidad de establecer determinados parámetros de calidad de servicio (QoS) para el almacenamiento en las máquinas virtuales. La QoS de almacenamiento permite aislar el rendimiento del almacenamiento en un entorno multiinquilino y proporciona mecanismos para notificar si el rendimiento de E/S del almacenamiento no cumple el umbral definido para ejecutar de forma eficaz las cargas de trabajo de las máquinas virtuales.

La QoS de almacenamiento permite especificar el número máximo de operaciones de E/S por segundo (IOPS) para tu disco duro virtual. Un administrador puede limitar el número de E/S de almacenamiento para impedir que un inquilino consuma demasiados recursos de almacenamiento de forma que afecte a otro inquilino.

También puede establecer un valor mínimo de IOPS. Se enviará una notificación cuando el número de IOPS de un disco duro virtual sea inferior al umbral necesario para un rendimiento óptimo.

La infraestructura de métricas de máquina virtual también se actualiza con parámetros relacionados con el almacenamiento para que el administrador pueda supervisar el rendimiento y anular los parámetros relacionados.

Los valores máximo y mínimo se especifican en términos de IOPS normalizada, donde cada 8 KB de datos se cuenta como una e/s.

Algunas de las limitaciones son las siguientes:

-   Solo para discos virtuales

-   El disco de diferenciación no puede tener un disco virtual primario en un volumen diferente

-   Réplica: QoS para el sitio de réplica configurada de forma independiente del sitio principal

-   No se admite VHDX compartido

Para obtener más información sobre la calidad de servicio del almacenamiento, consulte [calidad de servicio de almacenamiento para Hyper-V](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn282281(v=ws.11)).

### <a name="numa-io"></a>E/S DE NUMA

Windows Server 2012 y versiones posteriores admiten máquinas virtuales de gran tamaño, y cualquier configuración de máquina virtual de gran tamaño (por ejemplo, una configuración con Microsoft SQL Server que se ejecuten con procesadores virtuales 64) también necesitará escalabilidad en términos de rendimiento de e/s.

Las siguientes mejoras clave que se introdujeron por primera vez en la pila de almacenamiento de Windows Server 2012 e Hyper-V proporcionan las necesidades de escalabilidad de e/s de grandes máquinas virtuales:

-   Aumento en el número de canales de comunicación creados entre los dispositivos invitados y la pila de almacenamiento de host.

-   Un mecanismo de finalización de e/s más eficaz que implique la distribución de interrupciones entre los procesadores virtuales para evitar interrupciones de interprocesadores costosas.

Introducida en Windows Server 2012, hay algunas entradas del registro, ubicadas en HKLM \\ System \\ CurrentControlSet \\ enum \\ VMBUS \\ {Device ID} \\ {ID. \\ de instancia} StorChannel, que permiten ajustar el número de canales. También alinean los procesadores virtuales que controlan las finalizaciones de e/s con las CPU virtuales asignadas por la aplicación para que sean los procesadores de e/s. La configuración del registro se configura por cada adaptador en la clave de hardware del dispositivo.

-   **ChannelCount (DWORD)** Número total de canales que se van a usar, con un máximo de 16. Su valor predeterminado es un límite superior, que es el número de procesadores virtuales/16.

-   **ChannelMask (QWord)** La afinidad del procesador para los canales. Si no se establece o se establece en 0, el valor predeterminado es el algoritmo de distribución de canal existente que se usa para el almacenamiento normal o para los canales de red. Esto garantiza que los canales de almacenamiento no entren en conflicto con los canales de red.

### <a name="offloaded-data-transfer-integration"></a>Integración de Transferencia de datos descargado

Las tareas de mantenimiento cruciales para los VHD, como mezclar, trasladar y compactar dependen de la copia de grandes cantidades de datos. El método actual de copia de datos requiere que los datos se lean y escriban en ubicaciones diferentes, lo que puede ser un proceso lento. También utiliza recursos de CPU y memoria en el host, que podrían haberse usado para atender a las máquinas virtuales.

Los proveedores de red de área de almacenamiento (SAN) están trabajando para ofrecer operaciones de copia casi instantánea de grandes cantidades de datos. Este almacenamiento está diseñado para permitir que el sistema que está por encima de los discos especifique el movimiento de un conjunto de datos específico de una ubicación a otra. Esta característica de hardware se conoce como Transferencia de datos descargado.

Hyper-V en Windows Server 2012 y versiones posteriores admiten operaciones de descarga Transferencia de datos (ODX) para que estas operaciones puedan pasarse desde el sistema operativo invitado al hardware del host. Esto garantiza que la carga de trabajo puede usar el almacenamiento habilitado para ODX como si se estuviera ejecutando en un entorno no virtualizado. La pila de almacenamiento de Hyper-V también emite operaciones ODX durante operaciones de mantenimiento para discos duros virtuales como la combinación de discos y metaoperaciones de migración de almacenamiento donde se mueven grandes cantidades de datos.

### <a name="unmap-integration"></a>Desasignar la integración

Los archivos de disco duro virtual existen como archivos en un volumen de almacenamiento y comparten espacio disponible con otros archivos. Dado que el tamaño de estos archivos tiende a ser grande, el espacio que consume puede aumentar rápidamente. La demanda de más almacenamiento físico afecta al presupuesto de hardware de ti. Es importante optimizar el uso del almacenamiento físico tanto como sea posible.

Antes de Windows Server 2012, cuando las aplicaciones eliminan el contenido de un disco duro virtual, que ha abandonado el espacio de almacenamiento del contenido, la pila de almacenamiento de Windows del sistema operativo invitado y el host de Hyper-V tenían limitaciones que impidieron que esta información se comunicara al disco duro virtual y al dispositivo de almacenamiento físico. Esto impidió que la pila de almacenamiento de Hyper-V optimizara el uso de espacio por los archivos de disco virtual basados en VHD. También impide que el dispositivo de almacenamiento subyacente recupere el espacio que anteriormente ocupaba los datos eliminados.

A partir de Windows Server 2012, Hyper-V admite la desasignación de notificaciones, lo que permite que los archivos VHDX sean más eficientes en la representación de los datos que contiene. Esto da como resultado un tamaño de archivo más pequeño y permite que el dispositivo de almacenamiento físico subyacente reclame el espacio no utilizado.

Solo SCSI específico de Hyper-V, IDE habilitado y controladores de Canal de fibra virtuales permiten que el comando de desasignación del invitado llegue a la pila de almacenamiento virtual del host. En los discos duros virtuales, solo los discos virtuales formateados como VHDX admiten comandos de desasignación del invitado.

Por estos motivos, se recomienda usar archivos VHDX conectados a una controladora SCSI cuando no se usen discos Canal de fibra virtuales.

## <a name="additional-references"></a>Referencias adicionales

-   [Terminología de Hyper-V](terminology.md)

-   [Arquitectura de Hyper-V](architecture.md)

-   [Configuración del servidor de Hyper-V](configuration.md)

-   [Rendimiento del procesador de Hyper-V](processor-performance.md)

-   [Rendimiento del procesador de Hyper-V](memory-performance.md)

-   [Rendimiento de E/S de red de Hyper-V](network-io-performance.md)

-   [Detección de cuellos de botella en un entorno virtualizado.](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuales Linux](linux-virtual-machine-considerations.md)
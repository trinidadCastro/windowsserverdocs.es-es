---
title: Rendimiento de E/S de almacenamiento de Hyper-V
description: Consideraciones de rendimiento de e/s de almacenamiento en la optimización del rendimiento de Hyper-V
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: fedc23083914bcf97a8cde12b78c0b174143de25
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831316"
---
# <a name="hyper-v-storage-io-performance"></a>Rendimiento de E/S de almacenamiento de Hyper-V

Esta sección describen las distintas opciones y consideraciones para la optimización de rendimiento de E/S en una máquina virtual de almacenamiento. La ruta de acceso de E/S de almacenamiento se extiende desde la pila de almacenamiento de invitado a través de la capa de virtualización del host, en la pila de almacenamiento de host y, a continuación, en el disco físico. A continuación se muestran las explicaciones acerca de cómo las optimizaciones son posibles a cada una de estas fases.

## <a name="virtual-controllers"></a>Controladores virtuales

Hyper-V ofrece tres tipos de controladores virtuales: IDE, SCSI y virtuales, adaptadores de bus host (HBA).

## <a name="ide"></a>IDE

Controladoras IDE exponen discos IDE a la máquina virtual. La controladora IDE se emula y es el único controlador que está disponible para máquinas virtuales invitadas que ejecutan una versión anterior de Windows sin los servicios de integración de la máquina Virtual. E/S de disco que se realiza mediante el controlador de filtro IDE que se proporciona con los servicios de integración de la máquina Virtual es significativamente mayor que el rendimiento de E/S que se proporciona con la controladora IDE emulada de disco. Se recomienda utilizar discos IDE solo para los discos del sistema operativo porque tienen las limitaciones del rendimiento debido al tamaño de E/S máxima que se pueden emitir en estos dispositivos.

## <a name="scsi-sas-controller"></a>SCSI (controlador SAS)

Controladoras SCSI exponen discos SCSI a la máquina virtual, y cada controladora SCSI virtual puede admitir hasta 64 dispositivos. Para obtener un rendimiento óptimo, se recomienda que conectar varios discos a un único controlador SCSI virtual y crear controladores adicionales solo cuando sean necesarios para escalar el número de discos conectados a la máquina virtual. Ruta de acceso de SCSI no se emula lo que facilita el dispositivo preferido para cualquier disco que no sea el disco del sistema operativo. De hecho con las máquinas virtuales de generación 2 es el único tipo de controlador posibles. Se introdujo en Windows Server 2012 R2, este controlador se notifica como SAS para admitir el VHDX compartido.

## <a name="virtual-fibre-channel-hbas"></a>HBA de canal de fibra virtual

HBA de canal de fibra virtual puede configurarse para permitir el acceso directo para las máquinas virtuales al canal de fibra y canal de fibra sobre Ethernet (FCoE) LUN. Discos de canal de fibra virtual omiten el sistema de archivos NTFS en la partición raíz, lo que reduce el uso de CPU de E/S de almacenamiento.

Unidades de datos de gran tamaño y las unidades que se comparten entre varias máquinas virtuales (para los escenarios de clúster de invitado) son candidatos principales para los discos virtuales de canal de fibra.

Los discos de canal de fibra virtual requieren uno o varios adaptadores de bus de host (HBA) esté instalado en el host de canal de fibra. Es necesario usar un controlador HBA compatible con las capacidades de Windows Server 2016 Virtual Fibre Channel/NPIV cada host HBA. El tejido de SAN debe admitir NPIV, y los puertos HBA que se usan para el canal de fibra virtual se deben establecer en una topología de canal de fibra que admita NPIV.

Para maximizar el rendimiento en los hosts que se instalan con más de un HBA, se recomienda que configure varios HBA virtual dentro de la máquina virtual de Hyper-V (se puede configurar hasta cuatro HBA para cada máquina virtual). Hyper-V realizará automáticamente un mayor esfuerzo para equilibrar el HBA virtual para adaptadores HBA de host que tienen acceso a la misma SAN virtual.

## <a name="virtual-disks"></a>Discos virtuales

Los discos se pueden exponer a las máquinas virtuales a través de los controladores virtuales. Estos discos podrían ser discos duros virtuales que son abstracciones de archivo de un disco o un disco de acceso directo en el host.

## <a name="virtual-hard-disks"></a>Discos duros virtuales

Hay dos formatos de disco duro virtual, el VHD y VHDX. Cada uno de estos formatos admite tres tipos de archivos de disco duro virtual.

## <a name="vhd-format"></a>Formato de disco duro virtual

El formato de disco duro virtual es el formato de disco duro virtual sólo era compatible con Hyper-V en versiones anteriores. Se introdujo en Windows Server 2012, el formato de disco duro virtual se ha modificado para permitir una mejor alineación, que da como resultado un rendimiento significativamente mejor en los nuevos discos de sector grande.

Cualquier nuevo disco duro virtual que se crea en un equipo con Windows Server 2012 o posterior tiene la alineación de 4 KB óptimo. Este formato alineado es totalmente compatible con los sistemas operativos de Windows Server anteriores. Sin embargo, la propiedad de alineación, se interrumpirá para nuevas asignaciones de los analizadores que son compatibles con alineación como (un analizador de disco duro virtual desde una versión anterior de Windows Server) o un analizador que no son de Microsoft no 4 KB.

Cualquier disco duro virtual que se mueve desde una versión anterior no se convierten automáticamente a este nuevo formato mejorado de disco duro virtual.

Para convertir al nuevo formato VHD, ejecute el siguiente comando de Windows PowerShell:

``` syntax
Convert-VHD –Path E:\vms\testvhd\test.vhd –DestinationPath E:\vms\testvhd\test-converted.vhd
```

Puede comprobar la propiedad de alineación para todos los discos duros virtuales en el sistema y se debe convertir la alineación de 4 KB óptimo. Crear un nuevo disco duro virtual con los datos desde el disco duro virtual original mediante el **crear desde origen** opción.

Para comprobar si la alineación con Windows Powershell, examine la línea de alineación, tal como se muestra a continuación:

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

Para comprobar la alineación con Windows PowerShell, examine la línea de alineación, tal como se muestra a continuación:

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

VHDX es un nuevo formato de disco duro virtual que se introdujo en Windows Server 2012, que le permite crear resistentes discos virtuales de alto rendimiento hasta 64 terabytes. Ventajas de este formato son:

-   Compatibilidad con la capacidad de almacenamiento de disco duro virtual de hasta 64 terabytes.

-   Protección contra los daños en los datos durante las caídas del suministro eléctrico mediante el registro de las actualizaciones en las estructuras de metadatos de VHDX.

-   Capacidad de almacenar metadatos personalizados sobre un archivo, que un usuario quizás desee grabar, como la versión de sistema operativo o las revisiones aplicadas.

El formato VHDX también proporciona las siguientes ventajas de rendimiento:

-   Alineación mejorada del formato de disco duro virtual para que funcione bien en los discos de sector grande.

-   Mayor tamaño de bloque para los discos dinámicos y diferenciales, que permite que estos discos se adapten a las necesidades de la carga de trabajo.

-   Disco virtual de 4 KB sector lógico que permite aumenta el rendimiento cuando utilizados por aplicaciones y cargas de trabajo que están diseñadas para sectores de 4 KB.

-   Eficacia en la representación de datos, lo que da como resultado de tamaño de archivo y permite que el dispositivo de almacenamiento físico subyacente reclamar el espacio no utilizado. (El recorte requiere acceso directo o discos SCSI y hardware compatible con el recorte).

Al actualizar a Windows Server 2016, se recomienda convertir todos los archivos VHD para el formato VHDX debido a estas ventajas. El único escenario donde tendría sentido mantener los archivos en el formato de disco duro virtual es cuando una máquina virtual tiene la posibilidad de moverse a una versión anterior de Hyper-V que no es compatible con el formato VHDX.

## <a name="types-of-virtual-hard-disk-files"></a>Tipos de archivos de disco duro virtual

Hay tres tipos de archivos de disco duro virtual. Las secciones siguientes son las características de rendimiento y equilibrios entre los tipos.

Las siguientes recomendaciones deben tenerse en cuenta con respecto a la selección de un tipo de archivo de disco duro virtual:

-   Cuando se usa el formato VHD, se recomienda usar el tipo fijo porque tiene una mejor resistencia y características de rendimiento en comparación con los otros tipos de archivo de disco duro virtual.

-   Cuando se usa el formato VHDX, se recomienda usar el tipo dinámico porque ofrece resistencia además de ahorro de espacio que están asociado con asignar espacio solamente cuando es necesario hacerlo.

-   El tipo fijo también se recomienda, con independencia del formato, cuando el almacenamiento en el volumen de host no se supervisa activamente para garantizar que hay suficiente espacio en disco presente al expandir el archivo VHD en tiempo de ejecución.

-   Creación de instantáneas de una máquina virtual una diferenciación escrituras de disco duro virtual para almacenar en los discos. Tener solo algunas instantáneas pueden elevar el uso de CPU de almacenamiento de las operaciones de E/s, pero puede que no considerablemente al rendimiento excepto en las cargas de trabajo de servidor altamente E/s intensivas. Sin embargo, tener una gran cadena de instantáneas puede afectar notablemente al rendimiento porque la lectura desde el disco duro virtual puede requerir la comprobación de los bloques solicitados en muchos VHD de diferenciación. Es importante para mantener el rendimiento de E/S de disco en buen mantener las cadenas de instantánea corto.

## <a name="fixed-virtual-hard-disk-type"></a>Tipo de disco duro virtual fijo

En primer lugar se asigna espacio para el disco duro virtual cuando se crea el archivo VHD. Este tipo de archivo VHD es menos probable fragmentar, lo que reduce el rendimiento de E/S cuando se divide una E/S única en múltiples entradas y salidas. Tiene la menor sobrecarga de CPU de los tres tipos de archivo de disco duro virtual porque las lecturas y escrituras no es necesario buscar la asignación del bloque.

## <a name="dynamic-virtual-hard-disk-type"></a>Tipo de disco duro virtual dinámico

Espacio del disco duro virtual se asigna a petición. Los bloques en el disco de inicio como bloques sin asignar y no están respaldados por cualquier espacio real en el archivo. Primero vez que un bloque se escriben en, la pila de virtualización debe asignar espacio en el archivo de disco duro virtual para el bloque y, a continuación, actualizar los metadatos. Esto aumenta el número de operaciones de E/s de disco necesarios para la escritura y aumenta el uso de CPU. Lecturas y escrituras en los bloques existentes producen acceso al disco y la sobrecarga de CPU al buscar la asignación de los bloques en los metadatos.

## <a name="differencing-virtual-hard-disk-type"></a>Tipo de disco duro virtual de diferenciación

El disco duro virtual apunta a un archivo de disco duro virtual principal. Todas las escrituras en los bloques que no se escriben como resultado en el espacio que se asignó en el archivo de disco duro virtual, al igual que con un VHD de expansión dinámica. Las lecturas se atienden desde el archivo de disco duro virtual si el bloque se ha escrito en. En caso contrario, atiende desde el archivo de disco duro virtual primario. En ambos casos, los metadatos se leen para determinar la asignación del bloque. Lecturas y escrituras en este disco duro virtual pueden consumen más CPU y dar lugar a operaciones de E/s más que un archivo de disco duro virtual fijo.

## <a name="block-size-considerations"></a>Consideraciones de tamaño de bloque

Tamaño de bloque puede afectar significativamente al rendimiento. Resulta óptimo para que coincida con el tamaño de bloque para los patrones de asignación de la carga de trabajo que está usando el disco. Por ejemplo, si es la asignación de una aplicación en fragmentos de 16 MB, sería óptimo para tener un tamaño de bloque de disco duro virtual de 16 MB. Un tamaño de bloque de &gt;2 MB es posible únicamente en los discos duros virtuales con el formato VHDX. Tener un tamaño de bloque mayor que el modelo de asignación para una carga de trabajo de E/S aleatoria aumentará significativamente el uso del espacio en el host.

## <a name="sector-size-implications"></a>Implicaciones de tamaño de sector

La mayoría de la industria del software ha dependido de sectores de disco de 512 bytes, pero está moviendo el estándar en los sectores de disco de 4 KB. Para reducir los problemas de compatibilidad que pueden surgir de un cambio en el tamaño de sector, los proveedores de la unidad de disco duro están introduciendo un tamaño transitorio denominado unidades de emulación de 512 (512e).

Estas unidades de emulación ofrecen algunas de las ventajas que ofrecen 4 KB sector nativo unidades de disco, por ejemplo, mejor eficiencia de formato y un esquema mejorado para códigos de corrección de errores (ECC). Vienen con menos problemas de compatibilidad que se producirían al exponer un tamaño de sector de 4 KB en la interfaz del disco.

## <a name="support-for-512e-disks"></a>Compatibilidad con discos 512e

Un disco 512e puede realizar una operación de escritura solo con relación a un sector físico, es decir, no puede escribir directamente un sector de 512 bytes que se emite a él. El proceso interno en el disco que hace posible estas escrituras sigue estos pasos:

-   El disco lee el sector físico de 4 KB a su caché interna, que contiene el sector lógico de 512 bytes hace referencia en la operación de escritura.

-   Los datos en el búfer de 4 KB se modifican para incluir el sector de 512 bytes actualizado.

-   El disco realiza una operación de escritura del búfer de 4 KB actualizado a su sector físico en el disco.

Este proceso se denomina lectura-modificación-escritura (RMW). El impacto de rendimiento general del proceso RMW depende de las cargas de trabajo. El proceso RMW provoca una degradación del rendimiento en discos duros virtuales por las razones siguientes:

-   Los discos duros virtuales dinámicos y de diferencias tienen un mapa de bits del sector de 512 bytes frente a su carga de datos. Además, los localizadores primario, encabezado y pie de página alinea el elemento en un sector de 512 bytes. Es común para el controlador de disco duro virtual para emitir comandos de escritura de 512 bytes para actualizar estas estructuras, lo que resulta en el proceso RMW que se ha descrito anteriormente.

-   Las aplicaciones normalmente problema lecturas y escrituras en múltiplos de 4 KB de tamaño (el tamaño de clúster predeterminado de NTFS). Dado que hay un mapa de bits de sector de 512 bytes frente el bloque de carga de datos dinámico y discos duros virtuales de diferenciación, los bloques de 4 KB no se alinean con el límite físico de 4 KB. La siguiente ilustración muestra un disco duro virtual de 4 KB bloque (resaltado) que es no alineado con el límite físico de 4 KB.

![bloque de 4 kb de disco duro virtual](../../media/perftune-guide-vhd-4kb-block.png)

Cada comando de escritura de 4 KB emitida por el analizador actual para actualizar los datos de carga da como resultado dos lecturas para dos bloques del disco, que se actualizan y posteriormente volver a escribir en los dos bloques del disco. Hyper-V en Windows Server 2016 mitiga algunos de los efectos de rendimiento en los discos 512e sobre la pila de disco duro virtual mediante la preparación de las estructuras mencionadas anteriormente para la alineación con los límites de 4 KB en el formato VHD. Esto evita el efecto RMW al tener acceso a los datos dentro del archivo de disco duro virtual y al actualizar las estructuras de metadatos de disco duro virtual.

Como se mencionó anteriormente, los discos duros virtuales que se copian desde versiones anteriores de Windows Server no automáticamente se alineará con 4 KB. Puede convertirlos manualmente para alinear de forma óptima usando el **copia de origen** opción que está disponible en las interfaces VHD de disco.

De forma predeterminada, se exponen los discos duros virtuales con un tamaño de sector físico de 512 bytes. Esto se hace para garantizar que aplicaciones dependientes del tamaño de sector físico no se ven afectadas cuando la aplicación y los discos duros virtuales se mueven desde una versión anterior de Windows Server.

De forma predeterminada, se crean discos con el formato VHDX con el tamaño de sector físico de 4 KB para optimizar sus discos normales del perfil de rendimiento y discos de sector grande. Para hacer un uso completo de los sectores de 4 KB se recomienda para usar el formato VHDX.

## <a name="support-for-native-4kb-disks"></a>Compatibilidad con discos nativa de 4 KB

Hyper-V en Windows Server 2012 R2 y versiones posteriores de admite discos nativos de 4 KB. Pero es posible almacenar el disco VHD en discos nativos de 4 KB. Esto se hace implementando un software RMW algoritmo en la capa de pila de almacenamiento virtual que convierte las solicitudes de acceso y actualización de 512 bytes en 4 KB correspondientes accesos y las actualizaciones.

Dado que el archivo de disco duro virtual solo puede exponer a sí mismos como discos de tamaño de sector lógico de 512 bytes, es muy probable que haya aplicaciones que emiten solicitudes de E/S de 512 bytes. En estos casos, la capa RMW se cumplen estas solicitudes y provocar una degradación del rendimiento. Esto también es cierto para un disco que esté formateado con VHDX que tiene un tamaño de sector lógico de 512 bytes.

Es posible configurar un archivo VHDX se exponga como un disco de tamaño de sector lógico de 4 KB, y esto sería una configuración óptima para el rendimiento cuando el disco está hospedado en un dispositivo físico nativo de 4 KB. Debe tener cuidado para asegurarse de que el invitado y la aplicación que está usando el disco virtual están respaldadas por el tamaño de sector lógico de 4 KB. El formato de VHDX funcionará correctamente en un dispositivo de tamaño de sector lógico de 4 KB.

## <a name="pass-through-disks"></a>Discos de paso a través

El disco duro virtual en una máquina virtual puede asignarse directamente a un disco físico o el número de unidad lógica (LUN), en lugar de a un archivo VHD. La ventaja es que esta configuración omite el sistema de archivos NTFS en la partición raíz, lo que reduce el uso de CPU de E/S de almacenamiento. El riesgo es que los LUN o discos físicos puede ser más difícil mover entre equipos que los archivos de disco duro virtual.

Discos de paso a través se deben evitar debido a las limitaciones introducidas con escenarios de migración de máquina virtual.

## <a name="advanced-storage-features"></a>Características avanzadas de almacenamiento

### <a name="storage-quality-of-service-qos"></a>Calidad de servicio (QoS) del almacenamiento

A partir de Windows Server 2012 R2, Hyper-V incluye la capacidad de establecer determinados parámetros de calidad de servicio (QoS) para el almacenamiento en las máquinas virtuales. La QoS de almacenamiento permite aislar el rendimiento del almacenamiento en un entorno multiinquilino y proporciona mecanismos para notificar si el rendimiento de E/S del almacenamiento no cumple el umbral definido para ejecutar de forma eficaz las cargas de trabajo de las máquinas virtuales.

La QoS de almacenamiento permite especificar el número máximo de operaciones de E/S por segundo (IOPS) para tu disco duro virtual. Un administrador puede limitar el número de E/S de almacenamiento para impedir que un inquilino consuma demasiados recursos de almacenamiento de forma que afecte a otro inquilino.

También puede establecer un valor mínimo de IOPS. Se enviará una notificación cuando el número de IOPS de un disco duro virtual sea inferior al umbral necesario para un rendimiento óptimo.

La infraestructura de métricas de máquina virtual también se actualiza con parámetros relacionados con el almacenamiento para que el administrador pueda supervisar el rendimiento y anular los parámetros relacionados.

Valores máximos y mínimos se especifican en términos de IOPS normalizada, donde cada 8 KB de datos se cuenta como una E/S.

Algunas de las limitaciones son las siguientes:

-   Solo para los discos virtuales

-   Disco de diferenciación no puede tener el disco virtual principal en un volumen diferente

-   Réplica - QoS para el sitio de réplica que se configuran por separado desde el sitio primario

-   No se admite el VHDX compartido

Para obtener más información sobre la calidad de servicio de almacenamiento, consulte [calidad de servicio de almacenamiento para Hyper-V](https://technet.microsoft.com/library/dn282281.aspx).

### <a name="numa-io"></a>NUMA I/O

Windows Server 2012 y versiones posteriores admite grandes virtual machines y cualquier configuración de máquina virtual grande (por ejemplo, una configuración con Microsoft SQL Server con 64 procesadores virtuales) también necesitará la escalabilidad en términos de rendimiento de E/S.

Las siguientes mejoras clave que se introdujo por primera vez en la pila de almacenamiento de Windows Server 2012 e Hyper-V proporcionan las necesidades de escalabilidad de E/S de máquinas virtuales grandes:

-   Un aumento en el número de canales de comunicación entre los dispositivos de invitado y la pila de almacenamiento de host.

-   Un mecanismo de terminación E/S más eficaz que implican la distribución de interrupciones entre los procesadores virtuales para evitar costosas interrupciones interprocessor.

Se introdujo en Windows Server 2012, hay algunas entradas del registro, ubicadas en HKLM\\sistema\\CurrentControlSet\\Enum\\VMBUS\\{Id. de dispositivo}\\{Id. de instancia}\\StorChannel, que permiten el número de canales que se va a ajustar. También se alinean los procesadores virtuales que administran las finalizaciones de E/S a la CPU virtuales asignados por la aplicación para los procesadores de E/S. La configuración del registro se configura por adaptador en la clave de hardware del dispositivo.

-   **ChannelCount (DWORD)** el número total de canales para usar con un máximo de 16. El valor predeterminado es un límite superior, que es el número de procesadores virtuales o 16.

-   **ChannelMask (QWORD)** la afinidad del procesador para los canales. Si no está establecida o está establecida en 0, el valor predeterminado es el algoritmo de distribución de canal existente que se use para el almacenamiento normal o canales de interconexión. Esto garantiza que los canales de almacenamiento no entrará en conflicto con los canales de red.

### <a name="offloaded-data-transfer-integration"></a>Integración de transferencia de datos descarga

Tareas de mantenimiento fundamental de los discos duros virtuales, como combinar, mover y compacto, dependen de copiar grandes cantidades de datos. El método actual de copia de datos requiere que los datos se lean y escriban en ubicaciones diferentes, lo que puede ser un proceso lento. También se usa recursos de CPU y memoria en el host, que podría haber usado para las máquinas virtuales de servicio.

Los proveedores de red de área de almacenamiento (SAN) están trabajando para ofrecer operaciones de copia casi instantánea de grandes cantidades de datos. Este almacenamiento está diseñado para permitir que el sistema por encima de los discos para especificar el movimiento de un determinado conjunto de datos de una ubicación a otra. Esta característica de hardware se conoce como una transferencia de datos descargados.

Hyper-V en Windows Server 2012 y versiones posteriores admite operaciones de transferencia de datos de descarga (ODX) para que estas operaciones se pueden pasar desde el sistema operativo invitado al hardware del host. Esto garantiza que la carga de trabajo puede usar almacenamiento ODX habilitada como si se estuviera ejecutando en un entorno no virtualizado. La pila de almacenamiento de Hyper-V también emite operaciones ODX durante las operaciones de mantenimiento de discos duros virtuales como combinar discos y almacenamiento meta-operaciones de migración que se mueven grandes cantidades de datos.

### <a name="unmap-integration"></a>Anular la asignación de integración

Archivos de disco duro virtual existen como archivos en un volumen de almacenamiento y otros archivos que comparten espacio disponible. Dado que el tamaño de estos archivos se tiende a ser grandes, el espacio que consumen puede crecer rápidamente. Afecta a la demanda de almacenamiento físico más el presupuesto de hardware de TI. Es importante optimizar el uso de almacenamiento físico tanto como sea posible.

Antes de Windows Server 2012, cuando las aplicaciones eliminarán el contenido dentro de un disco duro virtual, lo que efectivamente abandona el espacio de almacenamiento del contenido, la pila de almacenamiento de Windows en el sistema operativo invitado y el host de Hyper-V tenía limitaciones que impedía esto información de que se comunican con el disco duro virtual y el dispositivo de almacenamiento físico. Esto impide que la pila de almacenamiento de Hyper-V optimizar el uso de espacio por los archivos de disco virtual basado en VHD. También impide que el dispositivo de almacenamiento subyacente reclame el espacio que anteriormente estaba ocupado por los datos eliminados.

A partir de Windows Server 2012, Hyper-V admite anular la asignación de notificaciones, que permiten los archivos VHDX ser más eficaz que representa los datos dentro de él. Esto da como resultado de tamaño de los archivos más pequeño, y permite que el dispositivo de almacenamiento físico subyacente reclamar el espacio no utilizado.

Habilitadas para solo específicos de Hyper-V SCSI, IDE y el canal de fibra Virtual controladores permite que el comando de unmap del invitado para llegar a la pila de almacenamiento virtual de host. En los discos duros virtuales, solo los discos virtuales con el formato VHDX admite anular la asignación de comandos desde el invitado.

Por estas razones, se recomienda que use archivos VHDX conectados a una controladora SCSI cuando no usa discos de canal de fibra Virtual.

## <a name="see-also"></a>Vea también

-   [Terminología de Hyper-v.](terminology.md)

-   [Arquitectura de Hyper-V](architecture.md)

-   [Configuración del servidor de Hyper-V](configuration.md)

-   [Rendimiento del procesador de Hyper-V](processor-performance.md)

-   [Rendimiento de la memoria de Hyper-V](memory-performance.md)

-   [Rendimiento de E/S de red de Hyper-V](network-io-performance.md)

-   [Detección de cuellos de botella en un entorno virtualizado.](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuales Linux](linux-virtual-machine-considerations.md)

---
title: Máquinas de virtuales de Ubuntu compatibles en Hyper-V
description: Enumera los servicios de integración de Linux y características incluidas en cada versión
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95ea5f7c-25c6-494b-8ffd-2a77f631ee94
author: shirgall
ms.author: shirgall
ms.date: 11/19/2018
ms.openlocfilehash: 662541658fe6e7b99e66fe31344450e0a1cbd201
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447831"
---
# <a name="supported-ubuntu-virtual-machines-on-hyper-v"></a>Máquinas de virtuales de Ubuntu compatibles en Hyper-V

>Se aplica a: Windows Server 2019, 2016, Hyper-V Server 2019, 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

A partir de Ubuntu 12.04, cargar el paquete "linux virtual" instala un kernel adecuado para su uso como una máquina virtual invitada. Este paquete siempre depende de la imagen mínima genérico del kernel más reciente y los encabezados que se utilizan para las máquinas virtuales. Aunque su uso es opcional, el kernel de linux virtual cargará controladores menos y puede iniciar más rápido y tiene menos sobrecarga de memoria que una imagen genérica.

Para obtener un uso completo de Hyper-V, instale los paquetes de herramientas de linux en la nube para instalar las herramientas y los demonios para su uso con máquinas virtuales y herramientas de linux adecuadas. Cuando se usa el kernel de linux virtual, cargar linux-tools-linux en la nube-tools-virtuales y.

El siguiente mapa de distribución de la característica indica las características de cada versión. Después de la tabla se enumeran los problemas conocidos y soluciones alternativas para cada distribución.

## <a name="table-legend"></a>Leyenda de la tabla

* **Integrado** -LIS se incluye como parte de esta distribución de Linux. El paquete de descarga LIS proporcionada por Microsoft no funciona para esta distribución, por lo que no lo instale. Los números de versión del módulo de kernel para la compilación en LIS (tal como se muestra por **lsmod**, por ejemplo) son diferentes desde el número de versión del paquete de descarga LIS proporcionada por Microsoft. Un error de coincidencia no indica que la compilación en LIS está obsoleta.

* &#10004;-Característica disponible

* (*en blanco*)-característica no está disponible

|**Característica**|**Versión del sistema operativo Windows Server**|**18.10**|**18.04 LTS**|**16.04 LTS**|**14.04 LTS**|**12.04 LTS**|
|-|-|-|-|-|-|-|
|**Disponibilidad**||Integrados|Integrados|Integrados|Integrados|Integrados|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Hora exacta de Windows Server 2016|2019, 2016|&#10004;|&#10004;|&#10004;|||
|**[Funciones de red](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|||||||
|Tramas gigantes|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Etiquetado de VLAN y enlace troncal|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Migración en vivo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Inserción de la dirección IP estática|2019, 2016, 2012 R2, 2012|&#10004;Nota 1|&#10004;Nota 1|&#10004;Nota 1|&#10004;Nota 1|&#10004;Nota 1|
|vRSS|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|Segmentación de TCP y las descargas de suma de comprobación|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019, 2016|&#10004;|&#10004;|&#10004;|||
|**[Storage](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||||||
|Cambio de tamaño de VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|Canal de fibra virtual|2019, 2016, 2012 R2|&#10004;Nota 2|&#10004;Nota 2|&#10004;Nota 2|&#10004;Nota 2||
|Copia de seguridad de máquina virtual en vivo|2019, 2016, 2012 R2|&#10004;Tenga en cuenta la 3, 4, 6|&#10004;Tenga en cuenta la 3, 4, 5|&#10004;Tenga en cuenta la 3, 4, 5|&#10004;Tenga en cuenta la 3, 4, 5||
|RECORTAR el soporte técnico|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|SCSI WWN|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**||||||
|Soporte técnico de núcleo PAE|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Configuración de gap MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Memoria dinámica - agregado en caliente|2019, 2016, 2012 R2, 2012|&#10004;Tenga en cuenta 7, 8, 9|&#10004;Tenga en cuenta 7, 8, 9|&#10004;Tenga en cuenta 7, 8, 9|&#10004;Tenga en cuenta 7, 8, 9||
|Memoria dinámica - incremento|2019, 2016, 2012 R2, 2012|&#10004;Tenga en cuenta 7, 8, 9|&#10004;Tenga en cuenta 7, 8, 9|&#10004;Tenga en cuenta 7, 8, 9|&#10004;Tenga en cuenta 7, 8, 9||
|Cambio de tamaño de memoria en tiempo de ejecución|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**|||||||
|Dispositivo de vídeo específico de Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Varios](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||||
|Par clave/valor|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;Nota 6, 10|&#10004;Nota 5, 10|&#10004;Nota 5, 10|&#10004;Nota 5, 10|&#10004;Nota 5, 10|
|Interrupción no enmascarable|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Copia de archivos de host a invitado|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|comando lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|Sockets de Hyper-V|2019, 2016||||||
|Acceso directo/DDA de PCI|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Máquinas virtuales de generación 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**||||||
|Arrancar con UEFI|2019, 2016, 2012 R2|&#10004;Tenga en cuenta 11, 12|&#10004;Tenga en cuenta 11, 12|&#10004;Tenga en cuenta 11, 12|&#10004;Tenga en cuenta 11, 12||
|Arranque seguro|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;||

## <a name="notes"></a>Notas

1. Inserción de IP estática puede no funcionar si **Network Manager** se ha configurado para un determinado adaptador de red específico de Hyper-V en la máquina virtual. Para garantizar un funcionamiento de la dirección IP estática inyección Asegúrese de que el Administrador de red está completamente desactivada o se ha desactivado para un adaptador de red específico a través de su **ifcfg ethX** archivo.

2. Al usar dispositivos de canal de fibra virtual, asegúrese de que se ha rellenado el número de unidad lógica (LUN 0) de 0. Si no se ha rellenado el LUN 0, es posible que una máquina virtual Linux no pueda montar de forma nativa los dispositivos de canal de fibra.

3. Si hay abierto identificadores de archivos durante una operación de copia de seguridad de máquina virtual activa, a continuación, en algunos casos excepcionales, la copia de seguridad de discos duros virtuales podrían tener que someterse a una comprobación de coherencia del sistema de archivos (`fsck`) en la restauración.

4. Operaciones de copia de seguridad en directo pueden fallar en modo silencioso si la máquina virtual tiene un dispositivo iSCSI conectados o almacenamiento conectado directo (también conocido como un disco de acceso directo).

5. Sobre la compatibilidad a largo plazo (LTS) versiones usar más reciente del kernel de habilitación de Hardware (HWE) virtual para Linux Integration Services al día.

   Para instalar el kernel de Azure ajustados en 14.04, 16.04 y 18.04, ejecute los comandos siguientes como raíz (sudo):

   ```bash
   # apt-get update
   # apt-get install linux-azure
   ```

   12.04 no tiene un núcleo virtual independiente. Para instalar el kernel HWE genérico en 12.04, ejecute los comandos siguientes como raíz (sudo):

   ```bash
   # apt-get update
   # apt-get install linux-generic-lts-trusty
   ```

   En Ubuntu 12.04 los demonios de Hyper-V siguientes están en un paquete instalado por separado:

   * **El demonio de instantánea de VSS** : este demonio es necesario para crear las copias de seguridad de máquina virtual en directo de Linux.
   * **El demonio KVP** : este demonio permite establecer y consultar pares clave-valor intrínsecos y extrínsecos.
   * **el demonio fcopy** : este demonio implementa un servicio entre el host e invitadas de copia de archivos.

   Para instalar el demonio KVP en 12.04, ejecute los comandos siguientes como raíz (sudo).

   ```bash
   # apt-get install hv-kvp-daemon-init linux-tools-lts-trusty linux-cloud-tools-generic-lts-trusty
   ```

   Cada vez que se actualiza el kernel, la máquina virtual debe reiniciarse para que lo utilicen.

6. En Ubuntu 18.10, use el kernel virtual más reciente para tener funcionalidades de Hyper-V actualizados.

   Para instalar el núcleo virtual en 18.10, ejecute los comandos siguientes como raíz (sudo):

   ```bash
   # apt-get update
   # apt-get install linux-azure
   ```

   Cada vez que se actualiza el kernel, la máquina virtual debe reiniciarse para que lo utilicen.

7. Compatibilidad con memoria dinámica sólo está disponible en las máquinas virtuales de 64 bits.

8. Las operaciones de memoria dinámicas pueden producir un error si el sistema operativo invitado se ejecuta demasiado bajo en memoria. Éstas son algunas prácticas recomendadas:

   * Memoria mínima y memoria de inicio deben ser igual o mayor que la cantidad de memoria que recomienda el proveedor de distribución.

   * Las aplicaciones que tienden a consumir toda la memoria disponible en un sistema se limitan a consumir hasta el 80 por ciento de memoria RAM disponible.

9. Si usa memoria dinámica en Windows Server 2019, Windows Server 2016 o los sistemas operativos Windows Server 2012/2012 R2, especifique **memoria de inicio**, **cantidad mínima de memoria**, y **máximo memoria** parámetros en múltiplos de 128 megabytes (MB). No hacerlo puede provocar errores de adición sin interrupción, y puede que no vea toda la memoria aumenta en un sistema operativo invitado.

10. En Windows Server 2019, Windows Server 2016 o Windows Server 2012 R2, la infraestructura de par clave/valor no podría funcionar correctamente sin una actualización de software de Linux. Póngase en contacto con su proveedor de distribución para obtener la actualización de software en caso de que vea problemas con esta característica.

11. En Windows Server 2012 R2, las máquinas virtuales de generación 2 tienen el arranque seguro habilitado de forma predeterminada y algunas máquinas virtuales no se iniciará a menos que la opción de arranque seguro está deshabilitada de Linux. Puede deshabilitar arranque seguro en el **Firmware** sección de la configuración de la máquina virtual en **Administrador de Hyper-V** o se puede deshabilitar con Powershell:

    ```Powershell
    Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off
    ```

12. Antes de intentar copiar el VHD de una máquina virtual de generación 2 VHD existente para crear nuevas máquinas virtuales de generación 2, siga estos pasos:

    1. Inicie sesión en la máquina virtual existente de generación 2.

    2. Cambie el directorio en el directorio de arranque EFI:

       ```bash
       # cd /boot/efi/EFI
       ```

    3. Copie el directorio de ubuntu en a un nuevo directorio denominado arranque:

       ```bash
       # sudo cp -r ubuntu/ boot
       ```

    4. Cambie el directorio al directorio de arranque recién creado:

       ```bash
       # cd boot
       ```

    5. Cambie el nombre del archivo shimx64.efi:

       ```bash
       # sudo mv shimx64.efi bootx64.efi
       ```

## <a name="see-also"></a>Vea también

* [Admite CentOS y Red Hat Enterprise Linux virtual machines en Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales Debian admitidas en Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuales Oracle Linux compatibles en Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuales SUSE compatibles en Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Descripciones de características para las máquinas virtuales de Linux y FreeBSD en Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Procedimientos recomendados para ejecutar Linux en Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Ubuntu 14.04 en una generación 2 VM - Blog de virtualización de Ben Armstrong](https://blogs.msdn.com/b/virtual_pc_guy/archive/2014/06/09/ubuntu-14-04-in-a-generation-2-vm.aspx)

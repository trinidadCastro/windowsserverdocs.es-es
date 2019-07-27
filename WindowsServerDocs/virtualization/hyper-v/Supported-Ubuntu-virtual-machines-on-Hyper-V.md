---
title: Máquinas virtuales de Ubuntu admitidas en Hyper-V
description: Enumera las características y servicios de integración de Linux que se incluyen en cada versión
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
ms.openlocfilehash: 8dad054e79a155e6aa3ba123aba4566f0f675a9d
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544692"
---
# <a name="supported-ubuntu-virtual-machines-on-hyper-v"></a>Máquinas virtuales de Ubuntu admitidas en Hyper-V

>Se aplica a: Windows Server 2019, 2016, Hyper-V Server 2019, 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

A partir de Ubuntu 12,04, al cargar el paquete "Linux-virtual" se instala un kernel adecuado para su uso como una máquina virtual invitada. Este paquete siempre depende de la imagen y los encabezados de kernel genéricos mínimos más recientes que se usan para las máquinas virtuales. Aunque su uso es opcional, el kernel virtual de Linux cargará menos controladores y podrá arrancar con mayor rapidez y tendrá menos sobrecarga de memoria que una imagen genérica.

Para obtener un uso completo de Hyper-V, instale los paquetes de herramientas de Linux y Linux-Cloud-Tools correspondientes para instalar herramientas y demonios para su uso con máquinas virtuales. Al usar el kernel virtual de Linux, cargue Linux-Tools-virtual y Linux-Cloud-Tools-virtual.

El siguiente mapa de distribución de características indica las características de cada versión. Los problemas conocidos y las soluciones alternativas para cada distribución se enumeran después de la tabla.

## <a name="table-legend"></a>Leyenda de tabla

* La **compilación en** LIS se incluye como parte de esta distribución de Linux. El paquete de descarga de LIS proporcionado por Microsoft no funciona para esta distribución, por lo que no se debe instalar. Los números de versión del módulo de kernel para el LIS integrado (como se muestra en **lsmod**, por ejemplo) son diferentes del número de versión del paquete de descarga de lis proporcionado por Microsoft. Una desigualdad no indica que el LIS integrado no está actualizado.

* &#10004;-Característica disponible

* (*en blanco*): característica no disponible

| **Característica**                                                                                                                                  | **Versión del sistema operativo Windows Server** | **19,04**             | **18,10**             | **18,04 LTS**         | **16,04 LTS**         | **14,04 LTS**         | **12,04 LTS**       |
|----------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------|-----------------------|-----------------------|-----------------------|-----------------------|-----------------------|---------------------|
| **Disponibilidad**                                                                                                                             |                                             | Integrados              | Integrados              | Integrados              | Integrados              | Integrados              | Integrados            |
| **[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**                                                   | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;            |
| Windows Server 2016 hora precisa                                                                                                            | 2019, 2016                                  | &#10004;              | &#10004;              | &#10004;              | &#10004;              |                       |                     |
| **[Soluciona](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**                                       |                                             |                       |                       |                       |                       |                       |                     |
| Tramas gigantes                                                                                                                                 | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;            |
| Etiquetado y Troncalización de VLAN                                                                                                                    | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;            |
| Migración en vivo                                                                                                                               | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;            |
| Inyección de IP estática                                                                                                                          | 2019, 2016, 2012 R2, 2012                   | &#10004;Nota 1       | &#10004;Nota 1       | &#10004;Nota 1       | &#10004;Nota 1       | &#10004;Nota 1       | &#10004;Nota 1     |
| vRSS                                                                                                                                         | 2019, 2016, 2012 R2                         | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;              |                     |
| Segmentación y descarga de sumas de comprobación TCP                                                                                                       | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;              |                     |
| SR-IOV                                                                                                                                       | 2019, 2016                                  | &#10004;              | &#10004;              | &#10004;              | &#10004;              |                       |                     |
| **[Discos](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**                                             |                                             |                       |                       |                       |                       |                       |                     |
| Cambiar el tamaño de VHDX                                                                                                                                  | 2019, 2016, 2012 R2                         | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;              |                     |
| Canal de fibra virtual                                                                                                                        | 2019, 2016, 2012 R2                         | &#10004;Nota 2       | &#10004;Nota 2       | &#10004;Nota 2       | &#10004;Nota 2       | &#10004;Nota 2       |                     |
| Copia de seguridad de máquinas virtuales en vivo                                                                                                                  | 2019, 2016, 2012 R2                         | &#10004;Nota 3, 4, 6 | &#10004;Nota 3, 4, 6 | &#10004;Nota 3, 4, 5 | &#10004;Nota 3, 4, 5 | &#10004;Nota 3, 4, 5 |                     |
| Compatibilidad con TRIM                                                                                                                                 | 2019, 2016, 2012 R2                         | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;            |
| WWN SCSI                                                                                                                                     | 2019, 2016, 2012 R2                         | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;              |                     |
| **[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**                                               |                                             |                       |                       |                       |                       |                       |                     |
| Compatibilidad con el kernel PAE                                                                                                                           | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;            |
| Configuración de la brecha de MMIO                                                                                                                    | 2019, 2016, 2012 R2                         | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;            |
| Memoria dinámica: agregar en caliente                                                                                                                     | 2019, 2016, 2012 R2, 2012                   | &#10004;Nota 7, 8, 9 | &#10004;Nota 7, 8, 9 | &#10004;Nota 7, 8, 9 | &#10004;Nota 7, 8, 9 | &#10004;Nota 7, 8, 9 |                     |
| Memoria dinámica: globos                                                                                                                  | 2019, 2016, 2012 R2, 2012                   | &#10004;Nota 7, 8, 9 | &#10004;Nota 7, 8, 9 | &#10004;Nota 7, 8, 9 | &#10004;Nota 7, 8, 9 | &#10004;Nota 7, 8, 9 |                     |
| Tamaño de memoria en tiempo de ejecución                                                                                                                        | 2019, 2016                                  | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;              |                     |
| **[Cámara](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**                                                 |                                             |                       |                       |                       |                       |                       |                     |
| Dispositivo de vídeo específico de Hyper-V                                                                                                                | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;              |                     |
| **[Varios](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**                                 |                                             |                       |                       |                       |                       |                       |                     |
| Par clave-valor                                                                                                                               | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;Nota 6, 10   | &#10004;Nota 6, 10   | &#10004;Nota 5, 10   | &#10004;Nota 5, 10   | &#10004;Nota 5, 10   | &#10004;Nota 5, 10 |
| Interrupción no enmascarable                                                                                                                       | 2019, 2016, 2012 R2                         | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;            |
| Copia de archivos de host a invitado                                                                                                                 | 2019, 2016, 2012 R2                         | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;              |                     |
| comando lsvmbus                                                                                                                              | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;              |                     |
| Sockets de Hyper-V                                                                                                                              | 2019, 2016                                  |                       |                       |                       |                       |                       |                     |
| Acceso directo/DDA de PCI                                                                                                                          | 2019, 2016                                  | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;              |                     |
| **[Máquinas virtuales de generación 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)** |                                             |                       |                       |                       |                       |                       |                     |
| Arranque mediante UEFI                                                                                                                              | 2019, 2016, 2012 R2                         | &#10004;Nota 11, 12  | &#10004;Nota 11, 12  | &#10004;Nota 11, 12  | &#10004;Nota 11, 12  | &#10004;Nota 11, 12  |                     |
| Arranque seguro                                                                                                                                  | 2019, 2016                                  | &#10004;              | &#10004;              | &#10004;              | &#10004;              | &#10004;              |                     |


## <a name="notes"></a>Notas

1. Es posible que la inyección de direcciones IP estáticas no funcione si se ha configurado el **Administrador de red** para un adaptador de red específico de Hyper-V determinado en la máquina virtual. Para garantizar el correcto funcionamiento de la inyección de IP estática, asegúrese de que el administrador de red esté desactivado completamente o se haya desactivado para un adaptador de red específico a través de su archivo **ifcfg-ethX** .

2. Al usar dispositivos de canal de fibra virtual, asegúrese de que se ha rellenado el número de unidad lógica 0 (LUN 0). Si LUN 0 no se ha rellenado, es posible que una máquina virtual de Linux no pueda montar los dispositivos de canal de fibra de forma nativa.

3. Si hay identificadores de archivos abiertos durante una operación de copia de seguridad de una máquina virtual activa, en algunos casos, es posible que los VHD de copia de seguridad deban someterse a una comprobación de coherencia del sistema de archivos (`fsck`) en la restauración.

4. Las operaciones de copia de seguridad en directo pueden producir errores silenciosamente si la máquina virtual tiene un dispositivo iSCSI conectado o un almacenamiento conectado directamente (también conocido como disco de acceso directo).

5. En las versiones de compatibilidad a largo plazo (LTS), se usa el kernel de habilitación de hardware virtual (HWE) más reciente para la Integration Services de Linux actualizada.

   Para instalar el kernel optimizado para Azure en 14,04, 16,04 y 18,04, ejecute los siguientes comandos como raíz (o sudo):

   ```bash
   # apt-get update
   # apt-get install linux-azure
   ```

   12,04 no tiene un kernel virtual independiente. Para instalar el kernel de HWE genérico en 12,04, ejecute los siguientes comandos como root (o sudo):

   ```bash
   # apt-get update
   # apt-get install linux-generic-lts-trusty
   ```

   En Ubuntu 12,04, los siguientes daemons de Hyper-V se encuentran en un paquete instalado por separado:

   * **Demonio de instantánea de VSS** : este demonio es necesario para crear copias de seguridad de máquinas virtuales de Linux en vivo.
   * **Demonio de kVp** : este demonio permite establecer y consultar los pares de valores de clave intrínsecos y extrínsecos.
   * **demonio fcopy** : este demonio implementa un servicio de copia de archivos entre el host y el invitado.

   Para instalar el demonio KVP en 12,04, ejecute los siguientes comandos como root (o sudo).

   ```bash
   # apt-get install hv-kvp-daemon-init linux-tools-lts-trusty linux-cloud-tools-generic-lts-trusty
   ```

   Cada vez que se actualiza el kernel, es necesario reiniciar la máquina virtual para usarla.

6. En Ubuntu 18,10 o 19,04, use el kernel virtual más reciente para tener funciones de Hyper-V actualizadas.

   Para instalar el kernel virtual en 18,10 o 19,04, ejecute los siguientes comandos como raíz (o sudo):

   ```bash
   # apt-get update
   # apt-get install linux-azure
   ```

   Cada vez que se actualiza el kernel, es necesario reiniciar la máquina virtual para usarla.

7. La compatibilidad con memoria dinámica solo está disponible en las máquinas virtuales de 64 bits.

8. Memoria dinámica operaciones pueden producir un error si el sistema operativo invitado se está ejecutando demasiado bajo en la memoria. A continuación se indican algunas prácticas recomendadas:

   * La memoria de inicio y la memoria mínima deben ser iguales o mayores que la cantidad de memoria que el proveedor de distribución recomienda.

   * Las aplicaciones que tienden a consumir toda la memoria disponible en un sistema se limitan a consumir hasta el 80 por ciento de la RAM disponible.

9. Si usa Memoria dinámica en los sistemas operativos Windows Server 2019, Windows Server 2016 o Windows Server 2012/2012 R2, especifique la **memoria de inicio**, la **memoria mínima**y los parámetros de **memoria máxima** en múltiplos de 128 megabytes (MB). Si no lo hace, pueden producirse errores de adición en caliente y es posible que no vea ningún aumento de la memoria en un sistema operativo invitado.

10. En Windows Server 2019, Windows Server 2016 o Windows Server 2012 R2, es posible que la infraestructura de pares clave-valor no funcione correctamente sin una actualización de software de Linux. Póngase en contacto con el proveedor de distribución para obtener la actualización de software en caso de que vea problemas con esta característica.

11. En Windows Server 2012 R2, las máquinas virtuales de generación 2 tienen el arranque seguro habilitado de forma predeterminada y algunas máquinas virtuales Linux no se iniciarán a menos que se deshabilite la opción de arranque seguro. Puede deshabilitar el arranque seguro en la sección **firmware** de la configuración de la máquina virtual en el **Administrador de Hyper-V** o puede deshabilitarla mediante PowerShell:

    ```Powershell
    Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off
    ```

12. Antes de intentar copiar el VHD de una máquina virtual VHD de generación 2 existente para crear nuevas máquinas virtuales de segunda generación, siga estos pasos:

    1. Inicie sesión en la máquina virtual de generación 2 existente.

    2. Cambie el directorio al directorio EFI de arranque:

       ```bash
       # cd /boot/efi/EFI
       ```

    3. Copie el directorio Ubuntu en un nuevo directorio denominado boot:

       ```bash
       # sudo cp -r ubuntu/ boot
       ```

    4. Cambie el directorio al directorio de arranque que acaba de crear:

       ```bash
       # cd boot
       ```

    5. Cambie el nombre del archivo shimx64. efi:

       ```bash
       # sudo mv shimx64.efi bootx64.efi
       ```

## <a name="see-also"></a>Vea también

* [Compatibilidad con máquinas virtuales de alta y Red Hat Enterprise Linux en Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales Debian admitidas en Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Oracle Linux compatibles con máquinas virtuales en Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales de SUSE compatibles en Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Descripciones de características de máquinas virtuales Linux y FreeBSD en Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Prácticas recomendadas para ejecutar Linux en Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Ubuntu 14,04 en una máquina virtual de generación 2: blog de virtualización de Ben Armstrong](https://blogs.msdn.com/b/virtual_pc_guy/archive/2014/06/09/ubuntu-14-04-in-a-generation-2-vm.aspx)

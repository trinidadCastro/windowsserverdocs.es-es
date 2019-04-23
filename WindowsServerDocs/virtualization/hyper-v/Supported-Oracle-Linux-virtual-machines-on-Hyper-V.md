---
title: Máquinas de virtuales Oracle Linux compatibles en Hyper-V
description: Enumera los servicios de integración de Linux y características incluidas en cada versión
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c02fdb5b-62f3-43cb-a190-ab74b3ebcf77
author: shirgall
ms.author: kathydav
ms.date: 06/01/2017
ms.openlocfilehash: a1569a30c0c5657de14c6df936b3b4596dceb7e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838706"
---
# <a name="supported-oracle-linux-virtual-machines-on-hyper-v"></a>Máquinas de virtuales Oracle Linux compatibles en Hyper-V

>Se aplica a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

El siguiente mapa de distribución de la característica indica las características que están presentes en cada versión. Después de la tabla se enumeran los problemas conocidos y soluciones alternativas para cada distribución.

En esta sección:

* [Serie de Kernel Compatible Red Hat](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md#BKMK_rhc)

* [Unbreakable Enterprise Kernel serie](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md#BKMK_uek)

* [Notas de la](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md#BKMK_notes)

## <a name="table-legend"></a>Leyenda de la tabla

* **Integrado** -LIS se incluye como parte de esta distribución de Linux. Los números de versión del módulo de kernel para la compilación en LIS (tal como se muestra por **lsmod**, por ejemplo) son diferentes desde el número de versión del paquete de descarga LIS proporcionada por Microsoft. Un error de coincidencia no indica que la compilación en LIS está obsoleta.

* &#10004;-Característica disponible

* (*en blanco*)-característica no está disponible

* **UEK R\*x QU\*y** -Unbreakable Enterprise Kernel (UEK) donde *x* es el número de versión y *y* es la actualización trimestral.

## <a name="BKMK_rhc"></a>Serie de Kernel Compatible Red Hat

El kernel de 32 bits para la serie 6.x es la característica PAE habilitada. No hay ninguna compatibilidad integrada de LIS para Oracle Linux RHCK 6.0-6.3. Kernels de Oracle Linux 7.x son solo 64 bits.

| **Característica**                                                                                                              | **Versión de Windows server**   | **7.5-7.6**         | **7.4**             | **6.4 6.8 y 7.0 a 7.3**                                             | **6.4 6.8 y 7.0 7.2**                                             | **RHCK 7.0-7.2**         | **6.8 RHCK**             | **RHCK 6.6, 6.7**        | **RHCK 6.5**              | **RHCK6.4**               |
|--------------------------------------------------------------------------------------------------------------------------|------------------------------|---------------------|---------------------|---------------------------------------------------------------------|---------------------------------------------------------------------|--------------------------|--------------------------|--------------------------|---------------------------|---------------------------|
| **Disponibilidad**                                                                                                         |                              | Integrado            | Integrado            | [LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106) | [LIS 4.1](https://www.microsoft.com/download/details.aspx?id=51612) | Integrado                 | Integrado                 | Integrado                 | Integrado                  | Integrado                  |
| **[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_core)**                          | 2016, 2012 R2, 2012, 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  |                           |
| Hora exacta de Windows Server 2016                                                                                        | 2016                         |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| **[Funciones de red](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Networking)**              |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| Tramas gigantes                                                                                                             | 2016, 2012 R2, 2012, 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| Etiquetado de VLAN y enlace troncal                                                                                                | 2016, 2012 R2, 2012, 2008 R2 | &#10004;            | &#10004;            | &#10004;(Nota 1 para 6.4 6.8)                                       | &#10004;(Nota 1 para 6.4 6.8)                                       | &#10004;                 | &#10004;Nota 1          | &#10004;Nota 1          | &#10004;Nota 1           | &#10004;Nota 1           |
| Migración en vivo                                                                                                           | 2016, 2012 R2, 2012, 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| Inserción de la dirección IP estática                                                                                                      | 2016, 2012 R2, 2012          | &#10004;Tenga en cuenta 14    | &#10004;Tenga en cuenta 14    | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| vRSS                                                                                                                     | 2016, 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 |                           |                           |
| Segmentación de TCP y las descargas de suma de comprobación                                                                                   | 2016, 2012 R2, 2012, 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 |                           |                           |
| SR-IOV                                                                                                                   | 2016                         | &#10004;            | &#10004;            |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| **[Storage](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Storage)**                    |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| Cambio de tamaño de VHDX                                                                                                              | 2016, 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  |                           |
| Canal de fibra virtual                                                                                                    | 2016, 2012 R2                | &#10004;Nota 2     | &#10004;Nota 2     | &#10004;Nota 2                                                     | &#10004;Nota 2                                                     | &#10004;Nota 2          | &#10004;Nota 2          | &#10004;Nota 2          | &#10004;Nota 2           |                           |
| Copia de seguridad de máquina virtual en vivo                                                                                              | 2016, 2012 R2                | &#10004;Tenga en cuenta 11,3  | &#10004;Tenga en cuenta 11, 3 | &#10004;Nota 3, 4                                                  | &#10004;Nota 3, 4                                                  | &#10004;Tenga en cuenta la 3, 4, 11   | &#10004;Tenga en cuenta la 3, 4, 11   | &#10004;Tenga en cuenta la 3, 4, 11   | &#10004;Tenga en cuenta la 3, 4, 5, 11 | &#10004;Tenga en cuenta la 3, 4, 5, 11 |
| RECORTAR el soporte técnico                                                                                                             | 2016, 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 |                          |                           |                           |
| SCSI WWN                                                                                                                 | 2016, 2012 R2                | &#10004;            |                     | &#10004;                                                            | &#10004;                                                            |                          |                          |                          |                           |                           |
| **[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Memory)**                      |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| Soporte técnico de núcleo PAE                                                                                                       | 2016, 2012 R2, 2012, 2008 R2 | N/D                 | N/D                 | &#10004;(solo 6.x)                                                 | &#10004;(solo 6.x)                                                 | N/D                      | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| Configuración de gap MMIO                                                                                                | 2016, 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| Memoria dinámica - agregado en caliente                                                                                                 | 2016, 2012 R2, 2012          | &#10004;Tenga en cuenta 8, 9  | &#10004;Tenga en cuenta 8, 9  | &#10004;Tenga en cuenta 7, 8, 9, 10 (Nota 6 para 6.4 6.7)                      | &#10004;Tenga en cuenta 7, 8, 9, 10 (Nota 6 para 6.4 6.7)                      | &#10004;Tenga en cuenta 6, 7, 8, 9 | &#10004;Tenga en cuenta 6, 7, 8, 9 | &#10004;Tenga en cuenta 6, 7, 8, 9 | &#10004;Tenga en cuenta 6, 7, 8, 9  |                           |
| Memoria dinámica - incremento                                                                                              | 2016, 2012 R2, 2012          | &#10004;Tenga en cuenta 8, 9  | &#10004;Tenga en cuenta 8, 9  | &#10004;Tenga en cuenta 7, 9, 10 (Nota 6 para 6.4 6.7)                         | &#10004;Tenga en cuenta 7, 9, 10 (Nota 6 para 6.4 6.7)                         | &#10004;Tenga en cuenta 6, 8, 9    | &#10004;Tenga en cuenta 6, 8, 9    | &#10004;Tenga en cuenta 6, 8, 9    | &#10004;Tenga en cuenta 6, 8, 9     | &#10004;Tenga en cuenta 6, 8, 9, 10 |
| Cambio de tamaño de memoria en tiempo de ejecución                                                                                                    | 2016                         |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| **[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Video)**                        |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| Dispositivo de vídeo específico de Hyper-V                                                                                            | 2016,2012 R2, 2012, 2008 R2  | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  |                           |
| **[Varios](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Misc)**                 |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| Par clave-valor                                                                                                           | 2016, 2012 R2, 2012, 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;Nota 12         | &#10004;Nota 12         | &#10004;Nota 12         | &#10004;Nota 12          | &#10004;Nota 12          |
| Interrupción no enmascarable                                                                                                   | 2016, 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| Copia de archivos de host a invitado                                                                                             | 2016, 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            |                          | &#10004;                 |                          |                           |                           |
| comando lsvmbus                                                                                                          | 2016, 2012 R2, 2012, 2008 R2 |                     |                     | &#10004;                                                            | &#10004;                                                            |                          |                          |                          |                           |                           |
| Sockets de Hyper-V                                                                                                          | 2016                         |                     |                     | &#10004;                                                            | &#10004;                                                            |                          |                          |                          |                           |                           |
| Acceso directo/DDA de PCI                                                                                                      | 2016                         | &#10004;            | &#10004;            |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| **[Máquinas virtuales de generación 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_gen2)** |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| Arrancar con UEFI                                                                                                          | 2016, 2012 R2                | &#10004;Tenga en cuenta 13    | &#10004;Tenga en cuenta 13    | &#10004;Tenga en cuenta 13                                                    | &#10004;Tenga en cuenta 13                                                    | &#10004;Tenga en cuenta 13         | &#10004;Tenga en cuenta 13         |                          |                           |                           |
| Arranque seguro                                                                                                              | 2016                         | &#10004;            | &#10004;            |                                                                     |                                                                     |                          |                          |                          |                           |                           |


## <a name="BKMK_uek"></a>Unbreakable Enterprise Kernel serie

Oracle Linux Unbreakable Enterprise Kernel (UEK) es sólo de 64 bits y dispone de LIS soporte integrado.

|**Característica**|**Versión de Windows server**|**R4 UEK**|**UEK R3 QU3**|**UEK R3 QU2**|**UEK R3 QU1**|
|-|-|-|-|-|-|
|**Disponibilidad**||Integrado|Integrado|Integrado|Integrado|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_core)**|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Hora exacta de Windows Server 2016|2016|||||
|**[Funciones de red](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Networking)**||||||
|Tramas gigantes|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Etiquetado de VLAN y enlace troncal|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Migración en vivo|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Inserción de la dirección IP estática|2016, 2012 R2, 2012|&#10004;|&#10004;|&#10004;||
|vRSS|2016, 2012 R2|&#10004;||||
|Segmentación de TCP y las descargas de suma de comprobación|2016, 2012 R2, 2012, 2008 R2|&#10004;||||
|SR-IOV|2016|||||
|**[Storage](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Storage)**||||||
|Cambio de tamaño de VHDX|2016, 2012 R2|&#10004;|&#10004;|&#10004;||
|Canal de fibra virtual|2016, 2012 R2|&#10004;|&#10004;|&#10004;||
|Copia de seguridad de máquina virtual en vivo|2016, 2012 R2|&#10004;Tenga en cuenta la 3, 4, 5, 11|&#10004;Tenga en cuenta la 3, 4, 5, 11|&#10004;Tenga en cuenta la 3, 4, 5, 11||
|RECORTAR el soporte técnico|2016, 2012 R2|&#10004;||||
|SCSI WWN|2016, 2012 R2|||||
|**[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Memory)**|
|Soporte técnico de núcleo PAE|2016, 2012 R2, 2012, 2008 R2|N/D|N/D|N/D|N/D|
|Configuración de gap MMIO|2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Memoria dinámica - agregado en caliente|2016, 2012 R2, 2012|&#10004;||||
|Memoria dinámica - incremento|2016, 2012 R2, 2012|&#10004;||||
|Cambio de tamaño de memoria en tiempo de ejecución|2016|||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Video)**||||||
|Dispositivo de vídeo específico de Hyper-V|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;||
|**[Varios](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Misc)**||||||
|Par clave-valor|2016, 2012 R2, 2012, 2008 R2|&#10004;Tenga en cuenta 11, 12|&#10004;Tenga en cuenta 11, 12|&#10004;Tenga en cuenta 11, 12|&#10004;Tenga en cuenta 11, 12|
|Interrupción no enmascarable|2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Copia de archivos de host a invitado|2016, 2012 R2|&#10004;Tenga en cuenta 11|&#10004;Tenga en cuenta 11|&#10004;Tenga en cuenta 11|&#10004;Tenga en cuenta 11|
|comando lsvmbus|2016, 2012 R2, 2012, 2008 R2|||||
|Sockets de Hyper-V|2016|||||
|Acceso directo/DDA de PCI|2016|||||
|**[Máquinas virtuales de generación 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_gen2)**|
|Arrancar con UEFI|2016, 2012 R2|&#10004;||||
|Arranque seguro|2016|&#10004;||||

## <a name="BKMK_notes"></a>Notas de la

1. Esta versión de Oracle Linux, funciona pero enlace troncal VLAN de etiquetado de VLAN no lo hace.

2. Al usar dispositivos de canal de fibra virtual, asegúrese de que se ha rellenado el número de unidad lógica (LUN 0) de 0. Si no se ha rellenado el LUN 0, es posible que una máquina virtual Linux no pueda montar de forma nativa los dispositivos de canal de fibra.

3. Si hay identificadores de archivos abiertos durante una operación de copia de seguridad de máquina virtual activa, a continuación, en algunos casos excepcionales, podrían tener los VHD de copia de seguridad que debe someterse a una comprobación de coherencia del sistema de archivo (fsck) en la restauración.

4. Operaciones de copia de seguridad en directo pueden fallar en modo silencioso si la máquina virtual tiene un dispositivo iSCSI conectados o almacenamiento conectado directo (también conocido como un disco de acceso directo).

5. Admite la copia de seguridad en vivo para Oracle Linux 6.4/6.5/UEKR3 QU2 y QU3 están disponibles a través de [aspectos básicos de copia de seguridad de Hyper-V para Linux](https://github.com/LIS/backupessentials/tree/1.0).

6. Compatibilidad con memoria dinámica sólo está disponible en las máquinas virtuales de 64 bits.

7. Soporte técnico en caliente no está habilitado de forma predeterminada en esta distribución. Para habilitar la compatibilidad de adición sin interrupción que tiene que agregar una regla udev en /etc/udev/rules.d/ como sigue:

   1. Cree un archivo **/etc/udev/rules.d/100-balloon.rules**. Puede usar cualquier otro nombre para el archivo deseado.

   2. En el archivo, agregue el siguiente contenido: `SUBSYSTEM=="memory", ACTION=="add", ATTR{state}="online"`

   3. Reinicie el sistema para habilitar la compatibilidad de adición sin interrupción.

   Mientras la descarga de Linux Integration Services crea esta regla en la instalación, la regla también se quita cuando se desinstala LIS, por lo que la regla se debe volver a crearse si se necesita memoria dinámica después de la desinstalación.

8. Las operaciones de memoria dinámica pueden producir un error si el sistema operativo invitado se ejecuta demasiado bajo en memoria. Éstas son algunas prácticas recomendadas:

   * Memoria mínima y memoria de inicio deben ser igual o mayor que la cantidad de memoria que recomienda el proveedor de distribución.

   * Las aplicaciones que tienden a consumir toda la memoria disponible en un sistema se limitan a consumir hasta el 80 por ciento de memoria RAM disponible.

9. Si usas la memoria dinámica en un sistema operativo Windows Server 2016 o Windows Server 2012 R2, especifique **memoria de inicio**, **cantidad mínima de memoria**, y **cantidad máxima de memoria** parámetros en múltiplos de 128 megabytes (MB). Si no lo hace puede dar lugar a errores en caliente y puede que no vea toda la memoria aumenta en un sistema operativo invitado.

10. Solo ciertas distribuciones, incluidas aquellas que usan LIS 3.5 o 4.0 de LIS, proporcionan soporte técnico de incremento y no proporcionan compatibilidad de adición sin interrupción. En este escenario, se puede usar la característica memoria dinámica estableciendo el parámetro de inicio de memoria en un valor que es igual que el parámetro de memoria máximo. Este resultado en toda la memoria necesaria está agregado en caliente a la máquina virtual en tiempo de arranque y, posteriormente, dependiendo de los requisitos de memoria del host, Hyper-V libremente puede asignar o desasignar memoria desde el invitado con el incremento. Configure **memoria de inicio** y **cantidad mínima de memoria** o mayor que el valor recomendado para la distribución.

11. Los demonios de Hyper-V de Linux de Oracle no se instalan de forma predeterminada. Para usar estos daemons, instale el paquete de demonios de Hyper-v. Este entra en conflicto con paquete descargado Linux Integration Services y no debe instalarse en sistemas con LIS descargado.

12. La infraestructura de clave/valor par (KVP) podría no funcionar correctamente sin una actualización de software de Linux. Póngase en contacto con su proveedor de distribución para obtener la actualización de software en caso de que vea problemas con esta característica.

13. Las máquinas virtuales de las plataformas Windows Server 2012 R2Generation 2 tiene el arranque seguro habilitado de forma predeterminada y algunas máquinas virtuales de Linux no se iniciará a menos que la opción de arranque seguro está deshabilitada. Puede deshabilitar arranque seguro en el **Firmware** sección de la configuración de la máquina virtual en **Administrador de Hyper-V** o se puede deshabilitar con Powershell:

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

    La descarga de Linux Integration Services se puede aplicar a las máquinas virtuales existentes generación 2 pero impartir la funcionalidad de generación 2.

14. Inserción de IP estática puede no funcionar si se ha configurado el Administrador de red para un adaptador de red sintético determinado en la máquina virtual. Para el correcto funcionamiento de la dirección IP estática inyección Asegúrese de que el Administrador de red está apagado completamente o se ha desactivado para un adaptador de red específico a través de su archivo ifcfg-ethX.


Vea también

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Admite CentOS y Red Hat Enterprise Linux virtual machines en Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Admite máquinas virtuales de Debian en Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuales SUSE compatibles en Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuales de Ubuntu compatibles en Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuales de FreeBSD compatibles en Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descripciones de características para las máquinas virtuales de Linux y FreeBSD en Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Procedimientos recomendados para ejecutar Linux en Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

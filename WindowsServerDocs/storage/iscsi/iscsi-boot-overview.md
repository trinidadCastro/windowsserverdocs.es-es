---
ms.assetid: 134840f3-c416-4a10-ad73-ef7855b206f7
title: Información general de arranque de destino iSCSI
description: 'Más información acerca de: Introducción al arranque de destino iSCSI'
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: 2bf8d2c3af86e23cb81d3d19d4dc0ee9c51c139d
ms.sourcegitcommit: 29b8942ea46196c12a67f6b6ad7f8dd46bf94fb2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98065641"
---
# <a name="iscsi-target-boot-overview"></a>Información general de arranque de destino iSCSI

> **Se aplica a:** Windows Server 2016

El Servidor de destino iSCSI en Windows Server puede arrancar cientos de equipos desde una sola imagen de sistema operativo que se almacena en una ubicación centralizada. Esto mejora la eficacia, la facilidad de uso, la disponibilidad y la seguridad.

## <a name="feature-description"></a><a name="BKMK_OVER"></a>Descripción de la característica
Mediante el uso de discos duros virtuales (VHD) de diferenciación, puede usar una sola imagen de sistema operativo (la "imagen maestra") para arrancar hasta 256 equipos. Por ejemplo, supongamos que ha implementado Windows Server con una imagen de sistema operativo de aproximadamente 20 GB y usó dos unidades de disco reflejadas para que actúen como el volumen de arranque. Hubiera necesitado aproximadamente 10 TB de almacenamiento solo para la imagen del sistema operativo para arrancar 256 equipos. Con el servidor de destino iSCSI, usará 40 GB para la imagen base de sistema operativo y 2 GB para los discos duros virtuales de diferenciación por cada instancia de servidor, lo que hace un total de 552 GB para las imágenes de sistema operativo. Esto permite ahorrar más del 90 % del almacenamiento solamente para las imágenes de sistema operativo.

## <a name="practical-applications"></a><a name="BKMK_APP"></a>Aplicaciones prácticas
El uso de una imagen de sistema operativo controlada ofrece las siguientes ventajas:

**Es más segura y fácil de administrar.** Algunas empresas necesitan proteger sus datos bloqueando físicamente el almacenamiento en una ubicación centralizada. En este escenario, los servidores obtienen acceso a los datos de forma remota, incluida la imagen de sistema operativo. Con el servidor de destino iSCSI, los administradores pueden administrar las imágenes de inicio del sistema operativo de forma centralizada y controlar las aplicaciones que se utilizarán para la imagen maestra.

**Implementación rápida.** Dado que la imagen maestra se prepara con Sysprep, cuando los equipos arrancan desde la imagen maestra, omiten la fase de copia e instalación del archivo que tiene lugar durante la instalación de Windows y pasan directamente a la fase de personalización. En pruebas internas de Microsoft, se implementaron 256 equipos en 34 minutos.

**Recuperación rápida.** Dado que las imágenes de sistema operativo se hospedan en el equipo que ejecuta el servidor de destino iSCSI, si es necesario reemplazar un cliente sin disco, el nuevo equipo puede apuntar a la imagen de sistema operativo y arrancar de inmediato.

> [!NOTE]
> Varios proveedores ofrecen una solución de arranque de red de área de almacenamiento (SAN), que el servidor de destino iSCSI puede usar en Windows Server en hardware estándar.

## <a name="hardware-requirements"></a><a name="BKMK_HARD"></a>Requisitos de hardware
El servidor de destino iSCSI no requiere hardware especial para la verificación funcional. En centros de datos con implementaciones en gran escala, el diseño debe validarse en relación con el hardware específico. Como referencia, las pruebas internas de Microsoft indicaron que una implementación de 256 equipos necesitaba 24 unidades de disco a 15.000 RPM en una configuración de RAID 10 para el almacenamiento. Un ancho de banda de red de 10 Gigabit es óptimo. Una estimación general es 60 servidores de arranque iSCSI por adaptador de red de 1 Gigabit.

En este escenario no se requiere un adaptador de red especializado y se puede usar un cargador de arranque de software (como el firmware de arranque de código abierto iPXE).

## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>Requisitos de software
El servidor de destino iSCSI se puede instalar como parte del servicio del rol Servicios de iSCSI y archivo en el administrador del servidor.

> [!NOTE]
> No se admite el arranque de Nano Server desde iSCSI (ya sea desde el Servidor de destino iSCSI de Windows o una implementación de destino de terceros).

## <a name="additional-references"></a>Referencias adicionales

* [Servidor de destino iSCSI](https://docs.microsoft.com/windows-server/storage/iscsi/iscsi-target-server)
* [iSCSI initiator cmdlets (Cmdlets de iniciador iSCSI)](https://docs.microsoft.com/powershell/module/iscsi/)
* [iSCSI Target Server cmdlets (Cmdlets de Servidor de destino iSCSI)](https://docs.microsoft.com/powershell/module/iscsitarget/)

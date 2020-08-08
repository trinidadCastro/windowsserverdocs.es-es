---
ms.assetid: 134840f3-c416-4a10-ad73-ef7855b206f7
title: Información general de arranque de destino iSCSI
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: f3f2ce63afa3e75b6d2f55789866c1872504c8eb
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87957422"
---
# <a name="iscsi-target-boot-overview"></a>Información general de arranque de destino iSCSI

> Se aplica a: Windows Server 2016

El Servidor de destino iSCSI en Windows Server puede arrancar cientos de equipos desde una sola imagen de sistema operativo que se almacena en una ubicación centralizada. Esto mejora la eficacia, la facilidad de uso, la disponibilidad y la seguridad.

## <a name="feature-description"></a><a name="BKMK_OVER"></a>Descripción de la característica
Mediante el uso de discos duros virtuales de diferenciación \( \) , puede usar una sola imagen \( de sistema operativo con la "imagen maestra" \) para arrancar hasta 256 equipos. Por ejemplo, supongamos que ha implementado Windows Server con una imagen de sistema operativo de aproximadamente 20 GB y usó dos unidades de disco reflejadas para que actúen como el volumen de arranque. Hubiera necesitado aproximadamente 10 TB de almacenamiento solo para la imagen del sistema operativo para arrancar 256 equipos. Con el servidor de destino iSCSI, usará 40 GB para la imagen base de sistema operativo y 2 GB para los discos duros virtuales de diferenciación por cada instancia de servidor, lo que hace un total de 552 GB para las imágenes de sistema operativo. Esto permite ahorrar más del 90 % del almacenamiento solamente para las imágenes de sistema operativo.

## <a name="practical-applications"></a><a name="BKMK_APP"></a>Aplicaciones prácticas
El uso de una imagen de sistema operativo controlada ofrece las siguientes ventajas:

**Es más segura y fácil de administrar.** Algunas empresas necesitan proteger sus datos bloqueando físicamente el almacenamiento en una ubicación centralizada. En este escenario, los servidores obtienen acceso a los datos de forma remota, incluida la imagen de sistema operativo. Con el servidor de destino iSCSI, los administradores pueden administrar las imágenes de inicio del sistema operativo de forma centralizada y controlar las aplicaciones que se utilizarán para la imagen maestra.

**Implementación rápida.** Dado que la imagen maestra se prepara con Sysprep, cuando los equipos arrancan desde la imagen maestra, omiten la fase de copia e instalación del archivo que tiene lugar durante la instalación de Windows y pasan directamente a la fase de personalización. En pruebas internas de Microsoft, se implementaron 256 equipos en 34 minutos.

**Recuperación rápida.** Como las imágenes de sistema operativo se hospedan en el equipo que ejecuta el servidor de destino iSCSI, si debe reemplazarse un cliente sin disco, el nuevo equipo puede seleccionar la imagen de sistema operativo y arrancar de inmediato.

> [!NOTE]
> Varios proveedores ofrecen una solución de arranque de red de área de almacenamiento \(SAN\), que el Servidor de destino iSCSI puede usar en Windows Server con hardware económico.

## <a name="hardware-requirements"></a><a name="BKMK_HARD"></a>Requisitos de hardware
El servidor de destino iSCSI no requiere hardware especial para la verificación funcional. En centros de datos con implementaciones en gran escala, el diseño debe validarse en relación con el hardware específico. Como referencia, las pruebas internas de Microsoft indicaron que una implementación de 256 equipos necesitaba 24 unidades de disco a 15.000 RPM en una configuración de RAID 10 para el almacenamiento. El ancho de banda óptimo es de 10 GB. El cálculo general es de servidores de arranque de 60 iSCSI por adaptador de red de 1 GB.

No se necesita un adaptador de red para este escenario y puede usarse un cargador de arranque de software \(como el firmware de arranque de código abierto de iPXE\).

## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>Requisitos de software
El servidor de destino iSCSI se puede instalar como parte del servicio del rol Servicios de iSCSI y archivo en el administrador del servidor.

> [!NOTE]
> No se admite el arranque de Nano Server desde iSCSI (ya sea desde el Servidor de destino iSCSI de Windows o una implementación de destino de terceros).

## <a name="additional-references"></a>Referencias adicionales
* [Servidor de destino iSCSI](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh848272(v=ws.11))
* [iSCSI initiator cmdlets (Cmdlets de iniciador iSCSI)](/powershell/module/iscsi/?view=win10-ps)
* [iSCSI Target Server cmdlets (Cmdlets de Servidor de destino iSCSI)](/powershell/module/iscsi/?view=win10-ps)

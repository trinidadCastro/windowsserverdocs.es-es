---
title: Las actualizaciones rápidas para volver a habilitar para noviembre de 2018 de Windows Server 2016 actualizar
description: Proporciona información acerca de las actualizaciones rápidas en Windows Server 2016
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.openlocfilehash: c48979440ab7c5cfa86aa1287b354a1e43692f48
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867446"
---
# <a name="express-updates-for-windows-server-2016-re-enabled-for-november-2018-update"></a>Las actualizaciones rápidas para volver a habilitar para noviembre de 2018 de Windows Server 2016 actualizar

>Por Joel Frauenheim

>Se aplica a: Windows Server 2016

Comenzando con el 13 de noviembre de 2018 el martes de actualización, Windows nuevo publicará las actualizaciones de Express para Windows Server 2016. Las actualizaciones rápidas de Windows Server 2016 se detienen en mediados de 2017 después de que se ha encontrado un problema significativo que mantienen las actualizaciones se instalen correctamente. Mientras que el problema se corrigió en noviembre de 2017, el equipo de actualización tardó un enfoque conservador a la publicación de los paquetes de Express para garantizar la mayoría de los clientes tendría la actualización del 14 de noviembre de 2017 ([4048953 KB](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) instalado en su servidor entornos y no se ve afectado por el problema.

Los administradores de sistema de WSUS y System Center Configuration Manager (SCCM) deben tener en cuenta que en noviembre de 2018 nuevamente verán dos paquetes de la actualización de Windows Server 2016: una actualización completa y una actualización de Express. Los administradores del sistema que quieran usar Express para sus entornos de servidor necesitan confirmar que el dispositivo ha realizado una actualización completa desde el 14 de noviembre de 2017 ([4048953 KB](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) para garantizar la actualización de Express se instala correctamente. Cualquier dispositivo que no se ha actualizado desde el 14 de noviembre de 2017 ([4048953 KB](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) verá errores repetidos que consumen ancho de banda y los recursos de CPU en un bucle infinito si se intenta realizar la actualización de Express.  Corrección para ese estado sería para que el administrador del sistema que detenga la inserción de la actualización de Express e insertar una reciente actualización completa para detener el bucle de error.

Con 13 de noviembre de 2018, los clientes de actualización de Express verán una reducción del tamaño de paquete entre su sistema de administración y los puntos de conexión de Windows Server 2016 inmediata.  
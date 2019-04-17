---
title: Las actualizaciones Express para volver a habilitar de noviembre de 2018 de Windows Server 2016 actualizar
description: Proporciona información acerca de las actualizaciones Express en Windows Server 2016
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.openlocfilehash: c48979440ab7c5cfa86aa1287b354a1e43692f48
ms.sourcegitcommit: 23e0a68e21985d709e029e7771d3c52d6815bcb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6507860"
---
# Las actualizaciones Express para volver a habilitar de noviembre de 2018 de Windows Server 2016 actualizar

>Por Joel Frauenheim

>Se aplica a: Windows Server2016

Iniciar con el 13 de noviembre de 2018 el martes de actualización, Windows se vuelve a publicar actualizaciones Express para Windows Server 2016. Express actualizaciones para Windows Server 2016 detenido de mediados de 2017, después de que se encontró un problema significativo que mantienen las actualizaciones se instalen correctamente. Mientras que el problema se solucionó en noviembre de 2017, el equipo de actualización realizó un enfoque conservador para publicar los paquetes de "Express" para garantizar que la mayoría de los clientes se tiene instalada en sus entornos de servidor la actualización del 14 de noviembre de 2017 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) y no se afectadas por el problema.

Los administradores de sistema para WSUS y System Center Configuration Manager (SCCM) deben tener en cuenta que en noviembre de 2018 una vez más verán dos paquetes de la actualización de Windows Server 2016: una actualización completa y una actualización de Express. Los administradores del sistema que quieren usar "Express" para sus entornos de servidor que confirma que el dispositivo ha tenido una actualización completa desde el 14 de noviembre de 2017 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) para garantizar que la actualización de Express se instala correctamente. Cualquier dispositivo que no se ha actualizado desde la actualización del 14 de noviembre de 2017 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) verán repite errores que consumen recursos de CPU en un bucle infinito y ancho de banda si se intenta realizar la actualización de Express.  Corrección para ese estado sería para que el administrador del sistema detener la actualización de Express que anima a y enviar una actualización completa recientes para detener el bucle de error.

Con el 13 de noviembre de 2018, los clientes de actualización de Express verán una reducción de tamaño del paquete entre los extremos de Windows Server 2016 y su sistema de administración inmediata.  
---
title: Actualizaciones rápidas para volver a habilitar Windows Server 2016 en la actualización de noviembre de 2018
description: Proporciona información acerca de las actualizaciones rápidas en Windows Server 2016
ms.topic: article
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.date: 08/07/2020
ms.openlocfilehash: e35f2d8c80ab31f85dc16d3b8f9b37af3ed105ed
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950321"
---
# <a name="express-updates-for-windows-server-2016-re-enabled-for-november-2018-update"></a>Actualizaciones rápidas para volver a habilitar Windows Server 2016 en la actualización de noviembre de 2018

> Se aplica a: Windows Server 2016

A partir de la actualización del martes, 13 de noviembre de 2018, Windows volverá a publicar las actualizaciones rápidas para Windows Server 2016. Las actualizaciones rápidas de Windows Server 2016 se detuvieron a mediados de 2017, después de que se encontrara un problema importante que impedía que las actualizaciones se instalaran correctamente. Si bien el problema se corrigió en noviembre de 2017, el equipo de actualización adoptó un enfoque conservador para la publicación de los paquetes rápidos, para garantizar que la mayoría de los clientes tuviese la actualización del 14 de noviembre de 2017 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) instalada en sus entornos de servidor y no se viera afectada por el problema.

Los administradores del sistema de WSUS y Configuration Manager deben tener en cuenta que en noviembre de 2018 volverán a ver dos paquetes de la actualización de Windows Server 2016: una actualización completa y una rápida. Los administradores del sistema que quieran usar la actualización rápida para sus entornos de servidor deben confirmar que el dispositivo ha hecho una actualización completa desde el 14 de noviembre de 2017 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) para garantizar que la actualización rápida se instale correctamente. Todo dispositivo que no se haya actualizado desde el 14 de noviembre de 2017 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) verá errores repetidos que consumen ancho de banda y recursos de CPU en un bucle infinito si se intenta realizar la actualización rápida.  Para corregir este estado, el administrador del sistema debe dejar de insertar la actualización rápida e insertar una reciente actualización completa para detener el bucle de errores.

Con la actualización rápida del 13 de noviembre de 2018, los clientes verán una reducción inmediata del tamaño de paquete entre su sistema de administración y los puntos de conexión de Windows Server 2016.
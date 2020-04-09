---
title: Mejoras en el administrador de memoria y caché
description: Mejoras del administrador de memoria y caché en Windows Server 2016
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: pavel; atales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: ef3658ab0f035435f6140c1dfa585de78537d37a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851648"
---
# <a name="cache-and-memory-manager-improvements"></a>Mejoras en el administrador de memoria y caché

En este tema se describen las mejoras en el administrador de caché y en el administrador de memoria de Windows Server 2012 y 2016.

## <a name="cache-manager-improvements-in-windows-server-2016"></a>Mejoras del administrador de caché en Windows Server 2016
El administrador de caché también agregó compatibilidad con las lecturas asincrónicas en caché verdaderas.
Esto podría mejorar el rendimiento de una aplicación si se basa en las lecturas asincrónicas almacenadas en caché.  Aunque la mayoría de los sistemas de archivos integrados han admitido lecturas en caché asincrónica durante un tiempo, a menudo había limitaciones de rendimiento debido a diversas opciones de diseño relacionadas con el control de los grupos de subprocesos y las colas de trabajo internas de los sistemas de archivos.  Con compatibilidad desde el kernel, el administrador de caché ahora oculta todas las complejidades de administración de colas de trabajos y grupos de subprocesos de los sistemas de archivos, lo que resulta más eficaz en el control de las lecturas asincrónicas almacenadas en caché. El administrador de caché tiene un conjunto de estructuras de control de memoria para cada uno de los niveles de anidamiento de VHD (máximo admitido por el sistema) para maximizar el paralelismo.


## <a name="cache-manager-improvements-in-windows-server-2012"></a>Mejoras del administrador de caché en Windows Server 2012
Además de las mejoras del administrador de caché para la lectura de la lógica de las cargas de trabajo secuenciales, se ha agregado un nuevo [CcSetReadAheadGranularityEx](https://msdn.microsoft.com/library/windows/hardware/hh406341.aspx) de API para permitir que los controladores del sistema de archivos, como SMB, cambien los parámetros de lectura previa. Permite mejorar el rendimiento de los escenarios de archivos remotos mediante el envío de varias solicitudes de lectura previa de pequeño tamaño en lugar de enviar una única solicitud de lectura previa grande. Solo los componentes del kernel, como los controladores del sistema de archivos, pueden configurar mediante programación estos valores por archivo.

## <a name="memory-manager-improvements-in-windows-server-2012"></a>Mejoras de Memory Manager en Windows Server 2012
Habilitar la combinación de páginas puede reducir el uso de memoria en los servidores que tienen muchas páginas privadas y paginables con contenido idéntico. Por ejemplo, los servidores que ejecutan varias instancias de la misma aplicación con un uso intensivo de memoria o una sola aplicación que funciona con datos muy repetitivos, podrían ser buenos candidatos para probar la combinación de páginas. El inconveniente de habilitar la combinación de páginas aumenta el uso de la CPU.

Estos son algunos ejemplos de roles de servidor en los que no es probable que la combinación de páginas ofrezca muchas ventajas:

-   Servidores de archivos (la mayoría de la memoria se utiliza en páginas de archivos que no son privadas y, por lo tanto, no se pueden combinar)

-   Servidores de Microsoft SQL Server que están configurados para usar AWE o páginas grandes (la mayor parte de la memoria es privada pero no paginable)

La combinación de páginas está deshabilitada de forma predeterminada, pero se puede habilitar mediante el cmdlet [enable-MMAgent de](https://technet.microsoft.com/library/jj658954.aspx) Windows PowerShell. La combinación de páginas se agregó en Windows Server 2012.

---
title: Mejoras del Administrador de memoria y caché
description: Memoria caché y las mejoras del Administrador de memoria en Windows Server 2016
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Pavel; ATales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: bd15cfc0714d1992e6b15d716b6b96ce259786da
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868586"
---
# <a name="cache-and-memory-manager-improvements"></a>Mejoras del Administrador de memoria y caché

Este tema describe las mejoras del Administrador de caché y el Administrador de memoria en Windows Server 2012 y 2016.

## <a name="cache-manager-improvements-in-windows-server-2016"></a>Mejoras del Administrador de caché en Windows Server 2016
Administrador de caché también se agregó compatibilidad para true asincrónica en caché lee.
Potencialmente, esto podría mejorar el rendimiento de una aplicación si depende en gran medida en caché de lectura asincrónicas.  Mientras más sistemas de archivos en el cuadro admitieron Lecturas asincrónicas almacenamiento en caché durante un tiempo, a menudo eran las limitaciones del rendimiento debido a diversas opciones de diseño relacionadas con la administración de grupos de subprocesos y colas de trabajo interno de sistemas de archivos.  Con el soporte de kernel adecuada, Administrador de caché ahora oculta todo el grupo de subprocesos y las complicaciones de administración de cola de trabajo de lo que más eficaz en el control asincrónico de sistemas de archivos en caché lecturas. Administrador de caché tiene conjuntos de control datastructures para cada uno de (máximo sistema compatible) niveles de anidamiento de disco duro virtual para maximizar el paralelismo.


## <a name="cache-manager-improvements-in-windows-server-2012"></a>Mejoras del Administrador de caché en Windows Server 2012
Además de mejoras en el Administrador de caché lectura anticipada lógica para cargas de trabajo secuenciales, una nueva API [CcSetReadAheadGranularityEx](https://msdn.microsoft.com/library/windows/hardware/hh406341.aspx) se agregó para permitir que los controladores del sistema, como SMB de archivos, cambiar sus parámetros lecturas anticipadas. Permite un mejor rendimiento para escenarios de archivo remoto mediante el envío de que solicitudes de varios pequeña lectura anticipada en lugar de enviar que una gran lectura previa solicitud. Sólo los componentes del kernel, como los controladores del sistema de archivos, pueden configurar mediante programación estos valores en una base por archivo.

## <a name="memory-manager-improvements-in-windows-server-2012"></a>Mejoras del Administrador de memoria en Windows Server 2012
Habilitar la combinación de página puede reducir el uso de memoria en los servidores que tiene una gran cantidad de páginas paginables privadas con contenido idéntico. Por ejemplo, los servidores que ejecutan varias instancias de la misma aplicación de gran cantidad de memoria o una única aplicación que funciona con datos muy repetitivos, puedan ser buenas candidatas para probar la combinación de página. La desventaja de la habilitación de la combinación de página es un mayor uso de CPU.

Estos son algunos ejemplos de roles de servidor donde página combinación no es probable que proporcionan muchas ventajas:

-   Servidores de archivos (las páginas del archivo que no son privados y, por tanto, no combinable consume la mayor parte de la memoria)

-   Servidores de Microsoft SQL Server que están configurados para utilizar AWE o páginas grandes (la mayoría de la memoria es privada, pero no paginable)

Combinación de página está deshabilitada de forma predeterminada, pero se puede habilitar mediante el uso de la [Enable MMAgent](https://technet.microsoft.com/library/jj658954.aspx) cmdlet de Windows PowerShell. Combinación de página se agregó en Windows Server 2012.

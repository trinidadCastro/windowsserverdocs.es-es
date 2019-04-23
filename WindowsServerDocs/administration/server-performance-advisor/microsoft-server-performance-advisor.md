---
title: Microsoft Server Performance Advisor
description: Microsoft Server Performance Advisor
ms.assetid: 468ebcb3-9eaf-477c-ab10-e3f1b3ce63dc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: manage
ms.openlocfilehash: ab124f3efabf2ac3ae8904157a81587c0c21ebb5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856336"
---
# <a name="microsoft-server-performance-advisor"></a>Microsoft Server Performance Advisor

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Descargue Microsoft Server Performance Advisor (SPA) para ayudar a diagnosticar problemas de rendimiento en Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o implementación de Windows Server 2008. SPA genera gráficos e informes de diagnóstico completos y proporciona recomendaciones para ayudarle a analizar problemas y desarrollar acciones correctivas.

-   [Información general de Server Performance Advisor](#bkmk-aboutspa)

-   [Descargue el Asesor de rendimiento de servidor](#bkmk-downloadspa)

-   [Guía de usuario de Server Performance Advisor](server-performance-advisor-users-guide.md)

-   [Guía de desarrollo de Server Performance Advisor Pack](server-performance-advisor-pack-development-guide.md)

## <a href="" id="bkmk-aboutspa"></a>Información general de Server Performance Advisor

Server Performance Advisor se compone de dos partes, el marco SPA y los módulos de Advisor SPA.

### <a name="the-server-performance-advisor-framework"></a>El marco de Server Performance Advisor

El motor que es responsable de la recopilación de datos que se designan mediante los módulos de advisor, escribir los datos recopilados en una base de datos de Microsoft SQL Server, crear un entorno compatible con TI para ejecutar los scripts para los módulos de SPA de Advisor y que muestra los informes de finales. Sólo necesitará instalar el marco SPA en la consola SPA. La consola SPA o bien puede instalarse en un equipo independiente para tener acceso a los servidores sometidos a prueba de forma remota o instalarse en un servidor sometida a prueba.

### <a name="server-performance-advisor-packs"></a>Paquetes de Server Performance Advisor

Módulos de SPA de Advisor son el centro de todas las reglas de optimización, que constan de una serie de metadatos y los archivos de script SQL. SPA se suministra con los siguientes módulos de Advisor:

-   El módulo de Core OS del Asesor de actualizaciones analiza el rendimiento de las funciones generales del sistema operativo, independientes de los roles de servidor especializado.

-   El módulo del Asesor de actualizaciones de Internet Information Server (IIS) realiza un seguimiento del rendimiento de IIS.

-   El módulo de Hyper-V Advisor analiza el rendimiento general del rol Hyper-V server.

    **Tenga en cuenta** el paquete del Asesor de Hyper-V no analiza los sistemas operativos invitados.

     

-   El módulo de Active Directory del Asesor de actualizaciones analiza el rendimiento general de la función active directory.

SPA también ofrece un modelo extensible para que no sean de Microsoft a los desarrolladores escribir módulos de advisor para adaptarlo a sus necesidades.

**Tenga en cuenta** SPA no entiende todos los contextos de escenario de usuario y del hardware. Debe usar las recomendaciones proporcionadas por la herramienta que le ayudarán a tomar decisiones y comprenda las consecuencias de los posibles cambios realizados en los servidores.

 

## <a href="" id="bkmk-downloadspa"></a>Descargue el Asesor de rendimiento de servidor


Use los siguientes vínculos para descargar Server Performance Advisor para Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008:

-   [Microsoft Server Performance Advisor 3.1 (32 bits)](https://go.microsoft.com/fwlink/p/?linkid=327751)

-   [Microsoft Server Performance Advisor 3.1 (64 bits)](https://go.microsoft.com/fwlink/p/?linkid=327752)

Puede extraer los archivos en el archivo CAB mediante los comandos siguientes:

-   para el x86 versión: `extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_x86.cab`

-   para el x64 versión: `extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_amd64.cab`

**Precaución** al extraer el archivo .cab, SPA debe conservar la estructura jerárquica de directorios para que funcione correctamente. Según las herramientas de CAB que se instalan en el servidor, puede producir la extracción en una estructura de directorios no operativa. Para conservar la estructura de directorios jerárquicos, puede usar una herramienta de utilidad de extracción de CAB que extrae una estructura de directorios de archivos.

Si la herramienta de extracción de CAB extraído correctamente los archivos, las subcarpetas aparecerán automáticamente en la carpeta de destino de extracción.

### <a name="spa-prerequisites"></a>Requisitos previos SPA

La consola de SPA requiere que esté instalado el software siguiente:

-   Microsoft .NET Framework 4

-   SQL Server 2008 R2 Express SP1 o Microsoft SQL Server 2008 R2 SP1

Las versiones más recientes pueden ser compatibles. Cualquier incompatibilidad conocidos de los productos con la consola de SPA se anotará.

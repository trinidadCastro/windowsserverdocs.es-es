---
title: Microsoft Server Performance Advisor
description: Microsoft Server Performance Advisor
ms.assetid: 468ebcb3-9eaf-477c-ab10-e3f1b3ce63dc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.topic: article
ms.openlocfilehash: 4ec0190c97c5afc761c27c7c3156380441951544
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895685"
---
# <a name="microsoft-server-performance-advisor"></a>Microsoft Server Performance Advisor

>Se aplica a: Windows Server (Canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Descargue Microsoft Server Performance Advisor (SPA) para ayudar a diagnosticar problemas de rendimiento en una implementación de Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008. SPA genera informes y gráficos de diagnóstico completos y proporciona recomendaciones que le ayudarán a analizar rápidamente los problemas y a desarrollar acciones correctivas.

-   [Información general de Server Performance Advisor](#bkmk-aboutspa)

-   [Descargar Server Performance Advisor](#bkmk-downloadspa)

-   [Guía de usuario de Server Performance Advisor](server-performance-advisor-users-guide.md)

-   [Guía de desarrollo de Server Performance Advisor Pack](server-performance-advisor-pack-development-guide.md)

## <a name="overview-of-server-performance-advisor"></a><a href="" id="bkmk-aboutspa"></a>Información general de Server Performance Advisor

Server Performance Advisor se compone de dos partes: el marco de SPA y los paquetes de asesores de SPA.

### <a name="the-server-performance-advisor-framework"></a>El marco de trabajo de Server Performance Advisor

El motor responsable de la recopilación de datos designados por los paquetes de asesores, la escritura de los datos recopilados en una base de datos de Microsoft SQL Server, la creación de un entorno de ti descriptivo para ejecutar scripts para los paquetes de asesores de SPA y la visualización de los informes finales. Solo tiene que instalar el marco de SPA en la consola de SPA. La consola de SPA puede instalarse en un equipo independiente para acceder de forma remota a los servidores en pruebas o instalarse en un servidor sometido a prueba.

### <a name="server-performance-advisor-packs"></a>Paquetes de Server Performance Advisor

Los paquetes de asesores de SPA son el centro de todas las reglas de optimización, que constan de una serie de metadatos y archivos de scripts SQL. SPA se incluye con los siguientes paquetes de Advisor:

-   El paquete principal de asesor de sistemas operativos analiza el rendimiento de las funciones generales del sistema operativo, independientemente de los roles de servidor especializados.

-   El paquete de asesor de Internet Information Server (IIS) realiza un seguimiento del rendimiento de IIS.

-   El paquete de asesor de Hyper-V analiza el rendimiento general del rol de servidor de Hyper-V.

    **Nota:** El paquete de asesor de Hyper-V no analiza los sistemas operativos invitados.



-   El módulo de asesor de Active Directory analiza el rendimiento general del rol de Active Directory.

SPA también ofrece un modelo extensible para que los desarrolladores que no son de Microsoft escriban paquetes de asesores que se adapten a sus necesidades.

**Nota:** SPA no puede comprender todos los contextos de hardware y de escenario de usuario. Debe usar las recomendaciones proporcionadas por la herramienta para ayudarle a tomar decisiones y entender las consecuencias de los posibles cambios que se realicen en los servidores.



## <a name="download-server-performance-advisor"></a><a href="" id="bkmk-downloadspa"></a>Descargar Server Performance Advisor


Use los vínculos siguientes para descargar Server Performance Advisor para Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008:

-   [Asesor de rendimiento de servidor de Microsoft 3,1 (32 bits)](https://go.microsoft.com/fwlink/p/?linkid=327751)

-   [Asesor de rendimiento de servidor de Microsoft 3,1 (64 bits)](https://go.microsoft.com/fwlink/p/?linkid=327752)

Puede extraer los archivos del archivo. CAB mediante los siguientes comandos:

-   para la versión x86:`extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_x86.cab`

-   para la versión x64:`extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_amd64.cab`

**PRECAUCIÓN** Al extraer el archivo. cab, SPA debe conservar la estructura de directorios jerárquica para que funcione correctamente. En función de las herramientas de archivo. CAB instaladas en el servidor, la extracción puede dar lugar a una estructura de directorios no operativa. Para conservar la estructura de directorios jerárquica, puede utilizar una herramienta de utilidad de extracción de archivos. CAB que extrae una estructura de directorio de archivos.

Si la herramienta de extracción de CAB extrajo los archivos correctamente, las subcarpetas aparecerán automáticamente en la carpeta de destino de extracción.

### <a name="spa-prerequisites"></a>Requisitos previos de SPA

La consola de SPA requiere que esté instalado el siguiente software:

-   Microsoft .NET Framework 4

-   SQL Server 2008 R2 Express SP1 o Microsoft SQL Server 2008 R2 SP1

Las versiones más recientes pueden ser compatibles. Se anotarán todas las incompatibilidades de productos conocidas con la consola de SPA.

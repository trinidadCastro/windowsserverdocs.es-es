---
title: Instalación y actualización de Windows Server
description: ''
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.date: 07/12/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98f876bd-63ff-4c3a-95d4-a8dd8d0d119c
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: c3b9070fc6cb9227ccfa445e23983d9e91fe5c82
ms.sourcegitcommit: 07ac08dea2b8f2763c2614a999dc7967018aa0b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "6121484"
---
# Instalación y actualización de Windows Server

>Se aplica a: WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2, Windows Server2008

> [!IMPORTANT]
> El soporte técnico ampliado para Windows Server 2008 R2 y Windows Server 2008 termina en enero de 2020. [Más información sobre las opciones de actualización](#upgrading-from-windows-server-2008-r2-or-windows-server-2008).

¿Ha llegado la hora de pasar a una versión más reciente de Windows Server? En función de la versión que estés utilizando en este momento, tienes muchas opciones para hacerlo.

## Instalación
Si deseas pasar a una versión más reciente de Windows Server en el mismo hardware, una forma que siempre funciona es una **instalación limpia**, en la que basta con instalar el sistema operativo más reciente directamente sobre el más antiguo en el mismo hardware, lo que elimina el sistema operativo anterior. Esta es la forma más sencilla, pero antes tendrás que hacer una copia de seguridad de los datos y planear la reinstalación de las aplicaciones. Hay algunas cosas que debes tener en cuenta, como los requisitos del sistema, así que asegúrate de consultar los detalles referentes a [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558), [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418) y [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).

El paso de cualquier versión preliminar (por ejemplo, Windows Server 2016 Technical Preview) a la versión de lanzamiento (Windows Server 2016) siempre requiere una instalación limpia.

## Migración (recomendado para Windows Server2016)

La documentación sobre la migración de Windows Server ayuda a migrar un rol o característica cada vez desde un equipo de origen que ejecute Windows Server a otro equipo de destino que ejecute Windows Server, ya sea la misma versión o una más reciente. Para estos fines, la migración se define como mover un rol o una característica y sus datos a un equipo diferente, no a actualizar la característica en el mismo equipo. Esta es la manera recomendada de mover los datos y la carga de trabajo existentes a una versión más reciente de Windows Server. Para empezar, consulta la [matriz de actualización y migración del rol de servidor](https://go.microsoft.com/fwlink/?LinkId=828595) de Windows Server 2016.

## Actualización del sistema operativo sucesiva de clúster
La actualización gradual del sistema operativo del clúster es una nueva característica de Windows Server 2016 que permite a un administrador actualizar el sistema operativo de los nodos del clúster de Windows Server 2012 R2 a Windows Server 2016 sin detener la función Hyper-V o las cargas de trabajo del Servidor de archivos de escalabilidad horizontal. Esta característica permite evitar tiempos de inactividad que podrían afectar a los contratos de nivel de servicio. Esta nueva característica se describe con más detalle en [Actualización del sistema operativo sucesiva de clúster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

## Conversión de licencia
En algunas versiones de los sistemas operativos, es posible convertir una edición concreta de la versión a otra edición de la misma versión en un solo paso, con un sencillo comando y la clave de licencia correspondiente. Esto se denomina **conversión de licencia**. Por ejemplo, si el servidor ejecuta Windows Server 2016 Standard, puedes realizar la conversión a Windows Server 2016 Datacenter. En algunas versiones de Windows Server, es posible convertir también libremente entre las versiones de OEM, de licencia por volumen y comerciales con el mismo comando y la clave apropiada.

## Actualización
Si quieres mantener el mismo hardware y todos los roles de servidor que hayas configurado sin eliminar el formato del servidor, una opción es la **actualización**, y existen muchas maneras de llevarla a cabo. En la actualización clásica, se pasa de un sistema operativo anterior a uno más reciente, y la configuración, los roles de servidor y los datos se mantienen intactos. Por ejemplo, si el servidor ejecuta Windows Server2012 R2, puedes actualizarlo a Windows Server 2016. Sin embargo, no todos los sistemas operativos antiguos tiene una ruta de actualización a todas las versiones más recientes.
 
>[!NOTE]
>La actualización funciona mejor en máquinas virtuales donde no se necesitan controladores de hardware específicos de OEM para que la actualización se efectúe correctamente.
 
Es posible actualizar de una versión de evaluación del sistema operativo a una versión comercial, de una versión comercial antigua a una nueva o, en algunos casos, de una edición de licencia por volumen a una edición comercial normal.

Antes de comenzar una actualización, echa un vistazo a las tablas que aparecen en esta página para ver cómo ir desde donde estés a donde quieras llegar.

Para obtener información acerca de las diferencias entre las opciones de instalación disponibles para Windows Server2016 Technical Preview, incluidas las características que se instalan con cada opción y las opciones de administración disponibles después de la instalación, consulta [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkId=828598).

>[!NOTE]
>Siempre que se migra o se actualiza a cualquier versión de Windows Server, es necesario revisar y comprender la [directiva de ciclos de vida de Microsoft](https://support.microsoft.com/lifecycle) y el período de tiempo para esa versión, y planificar en consecuencia. Puedes [buscar información sobre el ciclo de vida](https://support.microsoft.com/lifecycle) referente a la versión concreta de Windows Server que te interese.
 
 
## Actualización a WindowsServer2016
Para obtener información detallada, incluidas advertencias y limitaciones importantes aplicables a la actualización, la conversión de licencia entre ediciones de Windows Server 2016 y la conversión de ediciones de evaluación a la versión comercial, consulta [Opciones de actualización y conversión para Windows Server 2016](https://go.microsoft.com/fwlink/?LinkId=828602).
 
>[!NOTE]
>Nota: No se admiten las actualizaciones que cambien de una instalación Server Core a una instalación de Servidor con Experiencia de escritorio (o viceversa). Si el sistema operativo anterior que vas a actualizar o convertir es una instalación Server Core, el resultado seguirá siendo una instalación Server Core del sistema operativo más reciente.
 
Tabla de referencia rápida de rutas de actualización admitidas desde ediciones comerciales anteriores de Windows Server a ediciones comerciales de Windows Server2016:


|Si ejecutas estas versiones ediciones:|Puedes realizar una actualización a estas versiones y ediciones:|
|--------------------------------|---------------------------------------|
|Windows Server 2012 Standard|Windows Server 2016 Standard o Datacenter|
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard o Datacenter|
|Windows Server 2012R2 Datacenter|Windows Server 2016 Datacenter|
|Hyper-V Server 2012 R2|Hyper-V Server 2016 (con el uso de la característica de actualización sucesiva del sistema operativo del clúster)|
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server2012 Workgroup|Windows Storage Server 2016 Workgroup|
|Windows Storage Server2012R2 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 R2 Workgroup|Windows Storage Server 2016 Workgroup|
 
### Conversión de licencia
Puedes convertir Windows Server 2016 Standard (versión comercial) a Windows Server 2016 Datacenter (versión comercial).

Puedes convertir Windows Server 2016 Essentials (versión comercial) a Windows Server 2016 Standard (versión comercial).

Puedes convertir la versión de evaluación de Windows Server 2016 Standard a Windows Server 2016 Standard (versión comercial) o Datacenter (versión comercial).

Puedes convertir la versión de evaluación de Windows Server2016 Datacenter a Windows Server 2016 Datacenter (versión comercial).
 
## Actualización a WindowsServer2012 R2
Para obtener información detallada, incluidas advertencias y limitaciones importantes aplicables a la actualización, la conversión de licencia entre ediciones de Windows Server 2012 R2 y la conversión de ediciones de evaluación a la versión comercial, consulta [Opciones de actualización para Windows Server 2012 R2](https://technet.microsoft.com/library/dn303416.aspx).

Tabla de referencia rápida de rutas de actualización admitidas desde ediciones comerciales anteriores de Windows Server a ediciones comerciales de Windows Server2012 R2:

|Si ejecuta:|Puedes realizar una actualización a estas ediciones:|
|-------------------------|---------------------------|
|Windows Server2008R2 Datacenter con SP1|WindowsServer2012R2Datacenter|
|Windows Server2008R2 Enterprise con SP1|Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter|
|Windows Server2008R2 Standard con SP1|Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter|
|Windows Web Server2008R2 con SP1|Windows Server 2012 R2 Standard|
|Windows Server 2012 Datacenter|Windows Server 2012 R2 Datacenter|
|Windows Server 2012 Standard|Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter|
|Hyper-V Server 2012|Hyper-V Server 2012 R2|

### Conversión de licencia
Puedes convertir Windows Server 2012 Standard (versión comercial) a Windows Server 2012 Datacenter (versión comercial).

Puedes convertir Windows Server 2012 Essentials (versión comercial) a Windows Server 2012 Standard (versión comercial).

Puedes convertir la versión de evaluación de Windows Server 2012 Standard a Windows Server 2012 Standard (versión comercial) o Datacenter (versión comercial).

## Actualización a WindowsServer2012
Para obtener información detallada, incluidas advertencias y limitaciones importantes aplicables a la actualización y a la conversión de ediciones de evaluación a la versión comercial, consulta [Versiones de evaluación y opciones de actualización para Windows Server](https://technet.microsoft.com/library/jj574204.aspx).
 
Tabla de referencia rápida de rutas de actualización admitidas desde ediciones comerciales anteriores de Windows Server a ediciones comerciales de Windows Server2012:

|Si ejecuta:|Puedes realizar una actualización a estas ediciones:|
|--------------------------|--------------------------|
|Windows Server 2008 Standard con SP2 o Windows Server 2008 Enterprise con SP2|Windows Server 2012 Standard, Windows Server 2012 Datacenter|
|Windows Server 2008 Datacenter con SP2|Windows Server 2012 Datacenter|
|Windows Web Server 2008|Windows Server 2012 Standard|
|Windows Server2008R2 Standard con SP1 o Windows Server2008R2 Enterprise con SP1|Windows Server 2012 Standard, Windows Server 2012 Datacenter|
|Windows Server2008R2 Datacenter con SP1|WindowsServer2012Datacenter|
|Windows Web Server2008R2|Windows Server 2012 Standard|

### Conversión de licencia
Puedes convertir Windows Server 2012 Standard (versión comercial) a Windows Server 2012 Datacenter (versión comercial).

Puedes convertir Windows Server 2012 Essentials (versión comercial) a Windows Server 2012 Standard (versión comercial).

Puedes convertir la versión de evaluación de Windows Server 2012 Standard a Windows Server 2012 Standard (versión comercial) o Datacenter (versión comercial).

## La actualización de Windows Server 2008 R2 o Windows Server 2008

Como se describe en la [actualización de Windows Server 2008 y Windows Server 2008 R2](modernize-windows-server-2008.md), el servicio técnico ampliado para Windows Server 2008 R2 o Windows Server 2008 termina en enero de 2020. Para garantizar que ningún espacio en la compatibilidad, debes actualizar a una versión compatible de Windows Server, o realojar en Azure moviendo a [Especializada Windows Server 2008 R2 las máquinas virtuales](uploading-specialized-WS08-image-to-azure.md). Consulta la [Guía de migración de Windows Server](https://go.microsoft.com/fwlink/?linkid=872689) para obtener información y consideraciones para planear la migración y actualización.

Para servidores locales, no hay ninguna ruta de acceso directo de actualización de Windows Server 2008 R2 a Windows Server 2016 o posterior. En su lugar, en primer lugar actualizar a Windows Server 2012 R2 y, a continuación, [actualizar a Windows Server 2016](#Upgrading-to-Windows-Server-2016).

Como se planear la actualización, tener en cuenta las siguientes directrices para el paso intermedio de actualizar a Windows Server 2012 R2.

  - No se puede realizar una actualización en contexto de las arquitecturas de 32 bits a 64 bits o de una compilación tipo a otro (fre chk, por ejemplo).

  - Solo se admiten las actualizaciones en contexto en el mismo idioma. No se puede actualizar de un idioma a otro.

  - No se pueden migrar desde una instalación server core de Windows Server 2008 a Windows Server 2012 R2 con la GUI de servidor (denominado "Servidor con escritorio completo" en Windows Server). Puedes cambiar la instalación de servidor actualizado core al servidor con escritorio completa, pero solo en Windows Server 2012 R2. Windows Server 2016 y versiones posteriores admiten *no* cambiar desde el núcleo de servidor a todo el escritorio, por lo que debes ese conmutador antes de actualizar a Windows Server 2016.
  
Para obtener más información, echa un vistazo a [las versiones de evaluación y actualizar las opciones para Windows Server 2012](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574204\(v=ws.11\)), que incluye los detalles de actualización de funciones específicas.


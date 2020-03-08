---
title: Instalación y actualización de Windows Server
description: Cómo instalar, actualizar o migrar a una versión más reciente de Windows Server.
ms.prod: windows-server
ms.date: 05/14/2019
ms.technology: server-general
ms.topic: article
ms.assetid: 98f876bd-63ff-4c3a-95d4-a8dd8d0d119c
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 140f67a9dab5cf1f10cdb0c5c51a031a0dfb9dd3
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371595"
---
# <a name="windows-server-installation-and-upgrade"></a>Instalación y actualización de Windows Server

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

¿Buscas Windows Server 2019? Consulta [Instalar, actualizar o migrar a Windows Server 2019](../get-started-19/install-upgrade-migrate-19.md).

> [!IMPORTANT]
> El soporte técnico ampliado para Windows Server 2008 R2 y Windows Server 2008 termina en enero de 2020. [Obtén información sobre las opciones de actualización](#upgrading-from-windows-server-2008-r2-or-windows-server-2008).

¿Ha llegado la hora de pasar a una versión más reciente de Windows Server? En función de la versión que estés usando en este momento, tienes muchas opciones para hacerlo.

## <a name="installation"></a>Instalación

Si quieres pasar a una versión más reciente de Windows Server en el mismo hardware, una forma que siempre funciona es una **instalación limpia**, en la que basta con instalar el sistema operativo más reciente directamente sobre el más antiguo en el mismo hardware, lo que elimina el sistema operativo anterior. Esta es la forma más sencilla, pero antes tendrás que hacer una copia de seguridad de los datos y planear la reinstalación de las aplicaciones. Hay algunas cosas que debes tener en cuenta, como los requisitos del sistema, así que asegúrate de consultar los detalles referentes a [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558), [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418) y [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).

El paso de cualquier versión preliminar (por ejemplo, Windows Server 2016 Technical Preview) a la versión de lanzamiento (Windows Server 2016) siempre requiere una instalación limpia.

## <a name="migration-recommended-for-windows-server-2016"></a>Migración (recomendado para Windows Server 2016)

La documentación sobre la migración de Windows Server ayuda a migrar un rol o característica a la vez desde un equipo de origen que ejecute Windows Server a otro equipo de destino que ejecute Windows Server, ya sea la misma versión o una más reciente. Para tales fines, la migración se define como mover un rol o una característica y sus datos a un equipo diferente, no a actualizar la característica en el mismo equipo. Esta es la manera recomendada de mover los datos y la carga de trabajo existentes a una versión más reciente de Windows Server. Para empezar, consulta la [matriz de actualización y migración del rol de servidor](https://go.microsoft.com/fwlink/?LinkId=828595) de Windows Server.

## <a name="cluster-os-rolling-upgrade"></a>Actualización gradual del sistema operativo del clúster
La actualización gradual del sistema operativo del clúster es una nueva característica de Windows Server 2016 que permite a un administrador actualizar el sistema operativo de los nodos del clúster de Windows Server 2012 R2 a Windows Server 2016 sin detener la función Hyper-V ni las cargas de trabajo del Servidor de archivos de escalabilidad horizontal. Esta característica permite evitar tiempos de inactividad que podrían afectar a los contratos de nivel de servicio. Esta nueva característica se describe con más detalle en [Actualización gradual del sistema operativo del clúster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

## <a name="license-conversion"></a>Conversión de licencia
En algunas versiones de los sistemas operativos, es posible convertir una edición concreta de la versión a otra edición de la misma versión en un solo paso, con un sencillo comando y la clave de licencia correspondiente. Esto se denomina **conversión de licencia**. Por ejemplo, si el servidor ejecuta Windows Server 2016 Standard, puedes realizar la conversión a Windows Server 2016 Datacenter. En algunas versiones de Windows Server, es posible convertir también libremente entre las versiones de OEM, de licencia por volumen y comerciales con el mismo comando y la clave apropiada.

## <a name="upgrade"></a>Actualizar versión
Si quieres mantener el mismo hardware y todos los roles de servidor que hayas configurado sin eliminar el formato del servidor, una opción es la **actualización**, y existen muchas maneras de llevarla a cabo. En la actualización clásica, se pasa de un sistema operativo anterior a uno más reciente, y la configuración, los roles de servidor y los datos se mantienen intactos. Por ejemplo, si el servidor ejecuta Windows Server 2012 R2, puedes actualizarlo a Windows Server 2016. Sin embargo, no todos los sistemas operativos antiguos tienen una ruta de actualización a todas las versiones más recientes.
 
>[!NOTE]
>La actualización funciona mejor en máquinas virtuales donde los controladores de hardware específicos de OEM no son necesarios para una actualización correcta.
 
Es posible actualizar de una versión de evaluación del sistema operativo a una versión comercial, de una versión comercial antigua a una nueva o, en algunos casos, de una edición de licencia por volumen a una edición comercial normal.

Antes de comenzar una actualización, echa un vistazo a las tablas que aparecen en esta página para ver cómo ir desde donde estés a donde quieras llegar.

Para obtener información acerca de las diferencias entre las opciones de instalación disponibles para Windows Server 2016 Technical Preview, incluidas las características que se instalan con cada opción y las opciones de administración disponibles después de la instalación, consulta [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkId=828598).

>[!NOTE]
>Siempre que se migra o se actualiza a cualquier versión de Windows Server, es necesario revisar y comprender la [directiva de ciclos de vida de Microsoft](https://support.microsoft.com/lifecycle) y el período para esa versión, y planificar en consecuencia. Puedes [buscar información sobre el ciclo de vida](https://support.microsoft.com/lifecycle) referente a la versión concreta de Windows Server que te interese.
 
 
## <a name="upgrading-to-windows-server-2016"></a>Actualización a Windows Server 2016
Para obtener información detallada, incluidas advertencias y limitaciones importantes aplicables a la actualización, la conversión de licencia entre ediciones de Windows Server 2016 y la conversión de ediciones de evaluación a la versión comercial, consulta [Opciones de actualización y conversión para Windows Server 2016](https://go.microsoft.com/fwlink/?LinkId=828602).
 
>[!NOTE]
>Nota: No se admiten actualizaciones que cambian de la instalación Server Core al modo Servidor con Experiencia de escritorio (o viceversa). Si el sistema operativo anterior que vas a actualizar o convertir es una instalación básica, el resultado seguirá siendo una instalación básica del sistema operativo más reciente.
 
Tabla de referencia rápida de rutas de actualización admitidas desde ediciones comerciales anteriores de Windows Server a ediciones comerciales de Windows Server 2016:


|Si ejecutas estas versiones y ediciones:|Puedes actualizar a estas versiones y ediciones:|
|--------------------------------|---------------------------------------|
|Windows Server 2012 Standard|Windows Server 2016 Standard o Datacenter|
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard o Datacenter|
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter|
|Hyper-V Server 2012 R2|Hyper-V Server 2016 (con la característica de actualización gradual del sistema operativo del clúster)|
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 Workgroup|Windows Storage Server 2016 Workgroup|
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 R2 Workgroup|Windows Storage Server 2016 Workgroup|
 
### <a name="license-conversion"></a>Conversión de licencia
Puedes convertir Windows Server 2016 Standard (versión comercial) a Windows Server 2016 Datacenter (versión comercial).

Puedes convertir Windows Server 2016 Essentials (versión comercial) a Windows Server 2016 Standard (versión comercial).

Puede convertir la versión de evaluación de Windows Server 2016 Standard a Windows Server 2016 Standard (versión comercial) o Datacenter (versión comercial).

Puedes convertir la versión de evaluación de Windows Server 2016 Datacenter a Windows Server 2016 Datacenter (versión comercial).
 
## <a name="upgrading-to-windows-server-2012-r2"></a>Actualización a Windows Server 2012 R2
Para información detallada, incluidas advertencias y limitaciones importantes aplicables a la actualización, la conversión de licencia entre ediciones de Windows Server 2012 R2 y la conversión de ediciones de evaluación a la versión comercial, consulta [Opciones de actualización para Windows Server 2012 R2](https://technet.microsoft.com/library/dn303416.aspx).

Tabla de referencia rápida de rutas de actualización admitidas desde ediciones comerciales anteriores de Windows Server a ediciones comerciales de Windows Server 2012 R2:

|Si ejecuta:|Puede realizar una actualización a estas ediciones:|
|-------------------------|---------------------------|
|Windows Server 2008 R2 Datacenter con SP1|Windows Server 2012 R2 Datacenter|
|Windows Server 2008 R2 Enterprise con SP1|Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter|
|Windows Server 2008 R2 Standard con SP1|Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter|
|Windows Web Server 2008 R2 con SP1|Windows Server 2012 R2 Standard|
|Windows Server 2012 Datacenter|Windows Server 2012 R2 Datacenter|
|Windows Server 2012 Standard|Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter|
|Hyper-V Server 2012|Hyper-V Server 2012 R2|

### <a name="license-conversion"></a>Conversión de licencia
Puedes convertir Windows Server 2012 Standard (versión comercial) a Windows Server 2012 Datacenter (versión comercial).

Puedes convertir Windows Server 2012 Essentials (versión comercial) a Windows Server 2012 Standard (versión comercial).

Puede convertir la versión de evaluación de Windows Server 2012 Standard a Windows Server 2012 Standard (versión comercial) o Datacenter (versión comercial).

## <a name="upgrading-to-windows-server-2012"></a>Actualización a Windows Server 2012
Para información detallada, incluidas advertencias y limitaciones importantes aplicables a la actualización y a la conversión de ediciones de evaluación a la versión comercial, consulta [Versiones de evaluación y opciones de actualización para Windows Server 2012](https://technet.microsoft.com/library/jj574204.aspx).
 
Tabla de referencia rápida de rutas de actualización admitidas desde ediciones comerciales anteriores de Windows Server a ediciones comerciales de Windows Server 2012:

|Si ejecuta:|Puede realizar una actualización a estas ediciones:|
|--------------------------|--------------------------|
|Windows Server 2008 Standard con SP2 o Windows Server 2008 Enterprise con SP2|Windows Server 2012 Standard, Windows Server 2012 Datacenter|
|Windows Server 2008 Datacenter con SP2|Windows Server 2012 Datacenter|
|Windows Web Server 2008|Windows Server 2012 Standard|
|Windows Server 2008 R2 Standard con SP1 o Windows Server 2008 R2 Enterprise con SP1|Windows Server 2012 Standard, Windows Server 2012 Datacenter|
|Windows Server 2008 R2 Datacenter con SP1|Windows Server 2012 Datacenter|
|Windows Web Server 2008 R2|Windows Server 2012 Standard|

### <a name="license-conversion"></a>Conversión de licencia
Puedes convertir Windows Server 2012 Standard (versión comercial) a Windows Server 2012 Datacenter (versión comercial).

Puedes convertir Windows Server 2012 Essentials (versión comercial) a Windows Server 2012 Standard (versión comercial).

Puede convertir la versión de evaluación de Windows Server 2012 Standard a Windows Server 2012 Standard (versión comercial) o Datacenter (versión comercial).

## <a name="upgrading-from-windows-server-2008-r2-or-windows-server-2008"></a>Actualización desde Windows Server 2008 o Windows Server 2008 R2

Como se describe en [Actualizar Windows Server 2008 y Windows Server 2008 R2](modernize-windows-server-2008.md), el soporte técnico ampliado para Windows Server 2008 R2 y Windows Server 2008 finaliza en enero de 2020. Para asegurarte de no quedarte sin soporte técnico, tendrás que actualizar a una versión de Windows Server con soporte técnico, o rehospedar en Azure moviendo a [VM especializadas de Windows Server 2008 R2](uploading-specialized-WS08-image-to-azure.md). Consulta [Migration Guide for Windows Server](https://go.microsoft.com/fwlink/?linkid=872689) (Guía de migración para Windows Server) para información y consideraciones sobre la planeación de la migración o actualización.

Para servidores locales, no hay ninguna ruta de actualización directa de Windows Server 2008 R2 a Windows Server 2016 o posterior. En su lugar, actualiza primero a Windows Server 2012 R2 y, luego, [actualiza a Windows Server 2016](#upgrading-to-windows-server-2016).

Al planear la actualización, ten en cuenta las siguientes directrices para el paso intermedio de la actualización a Windows Server 2012 R2.

  - No puedes hacer una actualización en contexto desde las arquitecturas de 32 bits a 64 bits ni de un tipo de compilación a otro (de fre a chk, por ejemplo).

  - Solo se admiten actualizaciones en contexto en el mismo idioma. No puedes actualizar de un idioma a otro.

  - No puedes migrar desde una instalación básica de Windows Server 2008 a Windows Server 2012 R2 con la GUI de servidor (lo que se denomina "Servidor con escritorio completo" en Windows Server). Puedes cambiar la instalación básica actualizada a Servidor con escritorio completo, pero solo en Windows Server 2012 R2. Windows Server 2016 y versiones posterior *no* admiten el cambio de la instalación básica a escritorio completo, así que haz ese cambio antes de actualizar a Windows Server 2016.
  
Para más información, consulta [Versiones de evaluación y opciones de actualización para Windows Server 2012](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574204\(v=ws.11\)), que incluye detalles de actualización específicos para los roles.


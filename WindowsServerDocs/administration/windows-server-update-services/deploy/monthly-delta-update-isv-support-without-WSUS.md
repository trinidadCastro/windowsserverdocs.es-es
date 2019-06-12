---
title: Compatibilidad de ISV sin WSUS de actualizaciones de Delta mensual
description: Tema de Windows Server Update Service (WSUS) - cómo independientes de Software proveedores (ISV) puede usar temporalmente actualización mensual Delta en lugar de entrega de actualizaciones de WSUS Express para reducir el tamaño del paquete
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: dougkim
ms.date: 10/16/2017
ms.openlocfilehash: 272d3865bbe1a9853f5349c5e878155351525ef0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439941"
---
# <a name="monthly-delta-update-isv-support-without-wsus"></a>Compatibilidad de ISV sin WSUS de actualizaciones de Delta mensual

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10

Descargas de actualización de Windows 10 pueden ser grandes, porque cada paquete contiene todas las revisiones publicadas anteriormente para garantizar la coherencia y la simplicidad.  

Desde la versión 7, Windows ha sido capaz de reducir el tamaño de las descargas de Windows Update con una característica denominada [Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2), y aunque dispositivos para consumidores admiten de forma predeterminada, los dispositivos de Windows 10 enterprise requieren Windows Server Update Services (WSUS) para aprovechar las ventajas de Express. Si tiene WSUS disponibles, consulte [Express soporte para actualizaciones de entrega ISV](express-update-delivery-ISV-support.md). Se recomienda que utilice para permitir la entrega de actualizaciones de Express. 

Si no dispone actualmente WSUS instalado, pero más pequeñas, necesita actualizar los tamaños de paquete mientras tanto, puede usar la actualización mensual Delta. Actualización de delta reduce los tamaños de paquete notablemente, pero no tanto como con la entrega de actualización de WSUS Express. Le recomendamos que implemente la actualización de WSUS Express siempre que sea posible para la mayor reducción de los tamaños de paquete. Este es un gráfico que compare los tamaños de descarga de Delta, acumulados y Express para Windows 10 versión 1607:

![Comparación del tamaño de descarga](../../media/express-update-delivery-isv-support/delta-1.png)

## <a name="what-is-monthly-delta-update"></a>¿Qué es la actualización mensual?

Hay dos variantes de la actualización de seguridad mensual: Delta y acumulativas.

Actualización mensual del Delta es nuevo y una solución provisional para ISV que no tienen WSUS disponible para ayudar a reducir los tamaños de paquete de actualización.

>[!IMPORTANT]
>**Actualización de delta está disponible para el servicio de Windows 10, versión 1607 (actualización de aniversario), versión 1703 (Creators Update) y versión 1709 (Fall Creators Update).** Para las versiones después de la versión 1709, deberá implementar una infraestructura de implementación que admite [Express update entrega](express-update-delivery-ISV-support.md) para seguir disfrutando de las actualizaciones incrementales.

Mediante el uso de actualización mensual Delta, los paquetes sólo contendrá las actualizaciones de un mes. Acumulativos mensuales contiene todas las actualizaciones hasta esa versión de actualización, lo que resulta en un archivo grande que crece cada mes. Delta y actualizaciones mensuales se publican el segundo martes de cada mes, también conocido como "martes de actualización". La siguiente tabla compara las diferencias y las actualizaciones acumulativas:

|                    | Mensual **Delta** actualizar                                                                                                                                                                                                       | Mensual **acumulativa** actualizar                                                                                                                                                                                             |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ámbito**          | La actualización solo con **solo correcciones para ese mes**                                                                                                                                                                           | Actualización única con todas las nuevas correcciones para ese mes y todos los últimos meses                                                                                                                                                   |
| **Aplicación**    | Sólo puede aplicarse si actualización del mes anterior se ha aplicado (acumulados o diferencial)                                                                                                                                           | Se pueden aplicar en cualquier momento                                                                                                                                                                                                |
| **Entrega**       | **Publica únicamente a Windows Update Catalog** donde puede descargarse para su uso con otras herramientas o procesos. No se ofrece a los equipos que están conectados a Windows Update                                                         | Publicado en el catálogo de Windows Update, WSUS y actualización de Windows (donde todos los consumidores de los equipos lo instalará)                                                                                                                |

Delta y acumulativa tienen el mismo número KB, con la misma clasificación y liberar al mismo tiempo. Las actualizaciones se pueden distinguir por el título de actualización en el catálogo, o por el nombre de la msu:

- 2017-02 *\***actualización delta**\**  para Windows 10 versión 1607 para sistemas basados en x64 64 (KB1234567)
- 2017-02 *\***actualización acumulativa**\**  para Windows 10 versión 1607 para sistemas x86 (KB1234567)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      

### <a name="when-to-use-monthly-delta-update"></a>Cuándo usar la actualización mensual del Delta

Si el tamaño de la actualización en el dispositivo de cliente es un problema, se recomienda usar la actualización de diferencias en los dispositivos que tienen la actualización del mes anterior y actualización en los dispositivos que se retrasa acumulativa. De este modo, todos los dispositivos solo requieren una sola actualización ponerlos al día. Esto requiere un pequeño ajuste en el proceso general de administración de actualización, ya que tendrá que implementar actualizaciones diferentes según el grado de actualización de los dispositivos están en la organización:

![Comparación del tamaño de descarga](../../media/express-update-delivery-isv-support/delta-2.png)

### <a name="prevent-deployment-of-delta-and-cumulative-updates-in-the-same-month"></a>Evitar la implementación de actualizaciones acumulativa y Delta en el mismo mes

Puesto que la actualización de diferencias y la actualización acumulativa están disponibles al mismo tiempo, es importante entender lo que ocurre si implementa las dos actualizaciones en el mismo mes.

Si aprueba e implementa la misma versión del Delta y la actualización acumulativa, no solo generará tráfico de red adicional, ya que ambos se descargará en el equipo pero no es posible que pueda reiniciar el equipo para Windows después de reiniciar.

Si por accidente se instalan Delta y las actualizaciones acumulativas y ya no se arranca el equipo, puede recuperar con los pasos siguientes:

1. Arranque en el símbolo del sistema de WinRE
2. Enumerar los paquetes en un estado pendiente:

    `x:\windows\system32\dism.exe /image:<drive letter for windows directory> /Get-Packages >> <path to text file>`
 
    > **Ejemplo**: ` x:\windows\system32\dism.exe /image:c:\ /Get-Packages >> c:\temp\packages.txt`
 
3. Abra el archivo de texto donde canalizan **get-packages**. Si ve cualquier instalación pendiente revisiones, ejecute **remove-package** para cada nombre de paquete:
 
   `dism.exe /image:<drive letter for windows directory> /remove-package /packagename:<package name>`
 
    > **Ejemplo**: `x:\windows\system32\dism.exe /image:c:\ /remove-package /packagename:Package_for_KB4014329~31bf3856ad364e35~amd64~~10.0.1.0`
 
    >[!NOTE]
    >No quite pendiente revisiones de desinstalación.

>[!IMPORTANT]
>**Actualización de delta está disponible para el servicio de Windows 10, versión 1607 (actualización de aniversario), versión 1703 (Creators Update) y versión 1709 (Fall Creators Update).** Para las versiones después de la versión 1709, deberá implementar una infraestructura de implementación que admite [Express update entrega](express-update-delivery-ISV-support.md) para seguir disfrutando de las actualizaciones incrementales.

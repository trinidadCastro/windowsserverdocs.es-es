---
title: Compatibilidad de ISV de actualización Delta mensual sin WSUS
description: 'Tema de Windows Server Update Services (WSUS): cómo los fabricantes de software independientes (ISV) pueden usar temporalmente la actualización mensual Delta en lugar de la entrega de actualizaciones Express de WSUS para reducir el tamaño de los paquetes'
ms.topic: get-started article
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2b983b06a9f8bf4c2a6d5c72aef13689a72b3684
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89624472"
---
# <a name="monthly-delta-update-isv-support-without-wsus"></a>Compatibilidad de ISV de actualización Delta mensual sin WSUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016 y Windows 10

Las descargas de actualizaciones de Windows 10 pueden ser grandes, porque cada paquete contiene todas las revisiones publicadas anteriormente para garantizar la coherencia y simplicidad.

Desde la versión 7, Windows ha podido reducir el tamaño de las descargas de Windows Update con una característica llamada [Express](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc708456(v=ws.10)#Anchor_2) y, aunque los dispositivos de consumo la admiten de forma predeterminada, los dispositivos Windows 10 Enterprise requieren Windows Server Update Services (WSUS) para poder usarla. Si tienes WSUS disponible, consulta [Compatibilidad con ISV de entrega de actualizaciones Express](express-update-delivery-ISV-support.md). Se recomienda usarlo para habilitar la entrega de actualizaciones Express.

Si actualmente no tienes WSUS instalado, pero necesitas tamaños de paquete de actualización más pequeños mientras tanto, puedes usar la actualización mensual Delta. La actualización Delta reduce considerablemente los tamaños de los paquetes, pero no tanto como la entrega de actualizaciones Express de WSUS. Se recomienda implementar la actualización Express de WSUS siempre que sea posible para reducir al máximo los tamaños de los paquetes. A continuación se presenta un gráfico en el que se comparan los tamaños de descarga Express, Delta y Acumulativa para Windows 10, versión 1607:

![Comparación de tamaño de descarga](../../media/express-update-delivery-isv-support/delta-1.png)

## <a name="what-is-monthly-delta-update"></a>¿Qué es la actualización mensual Delta?

Hay dos variantes de la actualización de seguridad mensual: Delta y Acumulativa.

La actualización mensual Delta es nueva y es una solución provisional para los ISV que no tienen WSUS disponible para reducir los tamaños de los paquetes de actualización.

>[!IMPORTANT]
>**La actualización Delta está disponible para el servicio de Windows 10, versión 1607 (Actualización de aniversario), versión 1703 (Creators Update) y versión 1709 (Fall Creators Update).** En el caso de versiones posteriores a la 1709, tendrás que implementar una infraestructura de implementación que admita [la entrega de actualizaciones Express](express-update-delivery-ISV-support.md) para seguir aprovechando las actualizaciones incrementales.

Al usar la actualización mensual Delta, los paquetes solo contendrán actualizaciones de un mes. Las actualizaciones mensuales Acumulativas contienen todas las actualizaciones de la versión de actualización, lo que da lugar a un archivo que crece cada mes. Las actualizaciones Delta y mensuales se publican el segundo martes de cada mes, también conocido como martes de actualización. En la tabla siguiente se comparan las actualizaciones Delta y Acumulativa:

|                    | Actualización mensual **Delta**                                                                                                                                                                                                       | Actualización mensual **Acumulativa**                                                                                                                                                                                             |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ámbito**          | Actualización única con **solo nuevas correcciones para ese mes**                                                                                                                                                                           | Actualización única con todas las correcciones nuevas para ese mes y todos los meses anteriores                                                                                                                                                   |
| **Aplicación**    | Solo se puede aplicar si se aplicó la actualización del mes anterior (Acumulativa o Delta)                                                                                                                                           | Se puede aplicar en cualquier momento                                                                                                                                                                                                |
| **Entrega.**       | **Publicada solo en el catálogo de Windows Update**, donde se puede descargar para su uso con otras herramientas o procesos. No se ofrece a los equipos conectados a Windows Update                                                         | Publicada en Windows Update (desde donde la instalarán todos los equipos de consumo, WSUS y el catálogo de Windows Update                                                                                                                |

Las actualizaciones Delta y Acumulativa tienen el mismo número de KB, con la misma clasificación y versión al mismo tiempo. Las actualizaciones pueden distinguirse por el título de la actualización en el catálogo o por el nombre de la MSU:

- 02-2017 *\***Actualización Delta**\**   para Windows 10 Version 1607 para sistemas basados en x64 (KB1234567)
- 02-2017 *\***Actualización Acumulativa**\**   para Windows 10 Version 1607 para sistemas basados en x86 (KB1234567)

### <a name="when-to-use-monthly-delta-update"></a>Cuándo usar la actualización mensual Delta

Si el tamaño de la actualización en el dispositivo cliente es un problema, se recomienda usar la actualización Delta en los dispositivos que tienen la actualización del mes anterior y la actualización Acumulativa en los dispositivos que van por detrás. De esta manera, todos los dispositivos solo requieren una única actualización para estar al día. Esto requiere un pequeño ajuste en el proceso general de administración de actualizaciones, ya que tendrás que implementar diferentes actualizaciones en función de la actualización de los dispositivos de la organización:

![Comparación de tamaño de descarga](../../media/express-update-delivery-isv-support/delta-2.png)

### <a name="prevent-deployment-of-delta-and-cumulative-updates-in-the-same-month"></a>Impedir la implementación de actualizaciones Delta y Acumulativas en el mismo mes

Dado que la actualización Delta y la Acumulativa están disponibles al mismo tiempo, es importante comprender lo que ocurre si implementas ambas actualizaciones en el mismo mes.

Si apruebas e implementas la misma versión de la actualización Delta y Acumulativa, no solo generarás tráfico de red adicional, ya que ambas se descargarán en el equipo, sino que también es posible que no puedas arrancar el equipo en Windows después del reinicio.

Si se instalan las dos actualizaciones, Delta y Acumulativa, involuntariamente y el equipo ya no arranca, puedes seguir estos pasos para recuperarlo:

1. Arranca en el símbolo del sistema de WinRE
2. Muestra los paquetes con estado pendiente:

    `x:\windows\system32\dism.exe /image:<drive letter for windows directory> /Get-Packages >> <path to text file>`

    > **Ejemplo**: ` x:\windows\system32\dism.exe /image:c:\ /Get-Packages >> c:\temp\packages.txt`

3. Abre el archivo de texto donde canalizaste **get-packages**. Si ves alguna instalación con revisiones pendientes, ejecuta **remove-package** para cada nombre de paquete:

   `dism.exe /image:<drive letter for windows directory> /remove-package /packagename:<package name>`

    > **Ejemplo**: `x:\windows\system32\dism.exe /image:c:\ /remove-package /packagename:Package_for_KB4014329~31bf3856ad364e35~amd64~~10.0.1.0`

    >[!NOTE]
    >No quites las revisiones pendientes de desinstalación.

>[!IMPORTANT]
>**La actualización Delta está disponible para el servicio de Windows 10, versión 1607 (Actualización de aniversario), versión 1703 (Creators Update) y versión 1709 (Fall Creators Update).** En el caso de versiones posteriores a la 1709, tendrás que implementar una infraestructura de implementación que admita [la entrega de actualizaciones Express](express-update-delivery-ISV-support.md) para seguir aprovechando las actualizaciones incrementales.
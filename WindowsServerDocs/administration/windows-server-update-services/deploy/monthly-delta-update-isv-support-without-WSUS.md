---
title: Compatibilidad de ISV de actualización diferencial mensual sin WSUS
description: 'Tema de Windows Server Update Service (WSUS): cómo los fabricantes de software independientes (ISV) pueden usar temporalmente la actualización diferencial mensual en lugar de la entrega de actualizaciones Express de WSUS para reducir el tamaño de los paquetes'
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: dougkim
ms.date: 10/16/2017
ms.openlocfilehash: 4607827d73c34f50f721a2774fa498eb95f9dbb8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361732"
---
# <a name="monthly-delta-update-isv-support-without-wsus"></a>Compatibilidad de ISV de actualización diferencial mensual sin WSUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10

Las descargas de actualizaciones de Windows 10 pueden ser grandes porque cada paquete contiene todas las correcciones publicadas anteriormente para garantizar la coherencia y simplicidad.  

Desde la versión 7, Windows ha podido reducir el tamaño de las descargas de Windows Update con una característica denominada [Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2)y, aunque los dispositivos de consumidor lo admiten de forma predeterminada, los dispositivos de Windows 10 enterprise requieren Windows Server Update Services (WSUS) para realizar ventaja de Express. Si tiene WSUS disponible, consulte [compatibilidad con el ISV de entrega de actualizaciones rápidas](express-update-delivery-ISV-support.md). Se recomienda usarlo para habilitar la entrega rápida de actualizaciones. 

Si actualmente no tiene WSUS instalado, pero necesita tamaños de paquete de actualización más pequeños, puede usar la actualización diferencial mensual. La actualización diferencial reduce considerablemente los tamaños de los paquetes, pero no tanto como con la entrega de actualizaciones Express de WSUS. Se recomienda implementar la actualización de WSUS Express siempre que sea posible para reducir al máximo los tamaños de los paquetes. A continuación se presenta un gráfico en el que se comparan los tamaños de descarga rápida, Delta y acumulativa para Windows 10, versión 1607:

![Comparación de tamaño de descarga](../../media/express-update-delivery-isv-support/delta-1.png)

## <a name="what-is-monthly-delta-update"></a>¿Qué es la actualización diferencial mensual?

Hay dos variantes de la actualización de seguridad mensual: Delta y acum.

La actualización diferencial mensual es nueva y una solución provisional para los ISV que no tienen WSUS disponible para ayudar a reducir los tamaños de los paquetes de actualización.

>[!IMPORTANT]
>**La actualización diferencial está disponible para el servicio de Windows 10, versión 1607 (actualización de aniversario), versión 1703 (Creators Update) y versión 1709 (Fall Creators Update).** En el caso de las versiones posteriores a la versión 1709, tendrá que implementar una infraestructura de implementación que admita la entrega de actualizaciones [rápidas](express-update-delivery-ISV-support.md) para seguir aprovechando las actualizaciones incrementales.

Al usar la actualización diferencial mensual, los paquetes solo contendrán actualizaciones de un mes. Monthly Acum contiene todas las actualizaciones hasta la versión de actualización, lo que da lugar a un archivo grande que crece cada mes. Las actualizaciones Delta y mensual se publican el segundo martes de cada mes, también conocido como "actualizar martes". En la tabla siguiente se comparan las actualizaciones Delta y acumulativa:

|                    | Actualización **diferencial** mensual                                                                                                                                                                                                       | Actualización **acumulativa** mensual                                                                                                                                                                                             |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ámbito**          | Actualización única con **solo nuevas correcciones para ese mes**                                                                                                                                                                           | Actualización única con todas las correcciones nuevas para ese mes y todos los meses anteriores                                                                                                                                                   |
| **Aplicación**    | Solo se puede aplicar si se aplicó la actualización del mes anterior (acumulativa o delta)                                                                                                                                           | Se puede aplicar en cualquier momento                                                                                                                                                                                                |
| **Entrega**       | **Publicado solo en Windows Update Catálogo** , donde se puede descargar para su uso con otras herramientas o procesos. No se ofrece a los equipos conectados a Windows Update                                                         | Publicado en Windows Update (donde todos los equipos cliente lo instalarán), WSUS y el catálogo de Windows Update                                                                                                                |

Delta y Acum tienen el mismo número de KB, con la misma clasificación y versión al mismo tiempo. Las actualizaciones pueden distinguirse por el título de la actualización del catálogo o por el nombre de la MSU:

- 2017-02 *\*actualizacióndiferencial\*paraWindows 10*versión1607parasistemasbasados en x64 (KB1234567)
- 2017-02 *\*actualizaciónacumulativa\*paraWindows 10*versión1607parasistemasbasados en x86 (KB1234567)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      

### <a name="when-to-use-monthly-delta-update"></a>Cuándo usar la actualización diferencial mensual

Si el tamaño de la actualización en el dispositivo cliente es un problema, se recomienda usar la actualización diferencial en los dispositivos que tienen la actualización del mes anterior y la actualización acumulativa en los dispositivos que se encuentran detrás. De esta manera, todos los dispositivos solo requieren una única actualización para actualizarlos. Esto requiere un pequeño ajuste en el proceso general de administración de actualizaciones, ya que tendrá que implementar diferentes actualizaciones en función de la actualización de los dispositivos de la organización:

![Comparación de tamaño de descarga](../../media/express-update-delivery-isv-support/delta-2.png)

### <a name="prevent-deployment-of-delta-and-cumulative-updates-in-the-same-month"></a>Impedir la implementación de actualizaciones de diferencias y acumulativas en el mismo mes

Dado que la actualización diferencial y la actualización acumulativa están disponibles al mismo tiempo, es importante comprender lo que ocurre si implementa ambas actualizaciones en el mismo mes.

Si aprueba e implementa la misma versión de la actualización diferencial y acumulativa, no solo generará tráfico de red adicional ya que ambos se descargarán en el equipo, pero es posible que no pueda reiniciar el equipo en Windows después del reinicio.

Si las actualizaciones Delta y acumulativa se instalan de forma inadvertida y el equipo ya no arranca, puede realizar la recuperación con los pasos siguientes:

1. Arranque en el símbolo del sistema de WinRE
2. Enumerar los paquetes en un estado pendiente:

    `x:\windows\system32\dism.exe /image:<drive letter for windows directory> /Get-Packages >> <path to text file>`
 
    > **Ejemplo**:` x:\windows\system32\dism.exe /image:c:\ /Get-Packages >> c:\temp\packages.txt`
 
3. Abra el archivo de texto en el que ha canalizado **Get-Packages**. Si ve alguna revisión de instalación pendiente, ejecute **Remove-Package** para cada nombre de paquete:
 
   `dism.exe /image:<drive letter for windows directory> /remove-package /packagename:<package name>`
 
    > **Ejemplo**:`x:\windows\system32\dism.exe /image:c:\ /remove-package /packagename:Package_for_KB4014329~31bf3856ad364e35~amd64~~10.0.1.0`
 
    >[!NOTE]
    >No quite las revisiones pendientes de desinstalación.

>[!IMPORTANT]
>**La actualización diferencial está disponible para el servicio de Windows 10, versión 1607 (actualización de aniversario), versión 1703 (Creators Update) y versión 1709 (Fall Creators Update).** En el caso de las versiones posteriores a la versión 1709, tendrá que implementar una infraestructura de implementación que admita la entrega de actualizaciones [rápidas](express-update-delivery-ISV-support.md) para seguir aprovechando las actualizaciones incrementales.

---
title: Optimización del rendimiento para servidores web
description: Recomendaciones para la optimización del rendimiento para servidores web en Windows Server 16
ms.topic: landing-page
ms.author: ericam
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 79152179345dfe7b34a8e00ff2e4511970a647b3
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078072"
---
# <a name="performance-tuning-web-servers"></a>Optimización del rendimiento para servidores web


En este tema se describe los métodos de optimización del rendimiento y las recomendaciones para los servidores web de Windows Server 2016.


## <a name="selecting-the-proper-hardware-for-performance"></a>Selección del hardware adecuado para el rendimiento


Es importante seleccionar el hardware adecuado para satisfacer la carga web esperada, teniendo en cuenta la carga media, los picos de carga, la capacidad, los planes de crecimiento y los tiempos de respuesta. Los cuellos de botella de hardware limitan la eficacia del ajuste del software.

En [Performance Tuning for Server Hardware](../../hardware/index.md) (Optimización del rendimiento para hardware de servidor) se ofrecen recomendaciones de hardware a fin de evitar las restricciones de rendimiento siguientes:

-   Las CPU lentas ofrecen una capacidad de procesamiento limitada para las cargas de trabajo con uso intensivas de la CPU, como los escenarios de ASP, ASP.NET y TLS.

-   Una pequeña memoria caché de procesador de L2 o L3/LLC podría afectar al rendimiento.

-   Una cantidad limitada de memoria afecta al número de sitios que se pueden hospedar, cuántos scripts de contenido dinámico (por ejemplo, ASP.NET) pueden almacenarse, y el número de grupos de aplicaciones o procesos de trabajo.

-   Las redes se convierten en un cuello de botella debido a un adaptador de red ineficaz.

-   El sistema de archivos se convierte en un cuello de botella debido a la ineficacia de un adaptador de almacenamiento o de un subsistema de discos.

## <a name="operating-system-best-practices"></a>Procedimientos recomendados del sistema operativo


De ser posible, comience con una instalación limpia del sistema operativo. Actualizar el software puede dejar una configuración obsoleta, no deseada o poco óptima del registro, así como servicios y aplicaciones instalados previamente que consuman recursos si se inician automáticamente. Si hay instalado otro sistema operativo y debe conservarlo, debe instalar el nuevo sistema operativo en otra partición. En caso contrario, la nueva instalación sobrescribe la configuración en %Archivos de programa%\\Archivos comunes.

Para reducir las interferencias de acceso al disco, coloque el archivo de paginación del sistema, el sistema operativo, los datos web, la memoria caché de plantillas ASP y el registro de Internet Information Services (IIS) en discos físicos independientes, si es posible.

Para reducir la contención de recursos del sistema, instale Microsoft SQL Server e IIS en servidores diferentes, si es posible.

Evite instalar aplicaciones y servicios no esenciales. En algunos casos, podría ser conveniente deshabilitar servicios que no son obligatorios en un sistema.

## <a name="ntfs-file-system-settings"></a>Configuración del sistema de archivos NTFS

El conmutador global del sistema **NtfsDisableLastAccessUpdate** (REG\_DWORD) 1 se encuentra en **HKLM\\System\\CurrentControlSet\\Control\\FileSystem** y se establece en 1 de forma predeterminada. Este conmutador reduce la carga y las latencias de E/S del disco, al deshabilitar la actualización de la marca de fecha y tiempo para el último acceso al archivo o directorio. Las instalaciones limpias de Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008 habilitan esta configuración de manera predeterminada, y no es necesario ajustarla. Las versiones anteriores de Windows no establecían esta clave. Si el servidor ejecuta una versión anterior de Windows, o se ha actualizado a Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008, debe habilitar esta configuración.

Deshabilitar las actualizaciones es eficaz cuando se usan grandes conjuntos de datos (o varios hosts) que contienen miles de directorios. Se recomienda usar el registro de IIS en su lugar, si mantiene esta información solo para la administración web.

>[!Warning]
> Algunas aplicaciones, como las utilidades de copias de seguridad incrementales, dependen de esta información de actualización y no funcionan correctamente sin ella.

## <a name="additional-references"></a>Referencias adicionales
- [Optimización del rendimiento de IIS 10.0](tuning-iis-10.md)
- [Optimización de HTTP 1.1/2](http-performance.md)



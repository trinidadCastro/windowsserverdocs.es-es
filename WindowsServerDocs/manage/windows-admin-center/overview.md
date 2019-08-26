---
title: Introducción a Windows Admin Center
description: Aprenda a administrar Windows Server con Windows Admin Center (Proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 08/22/2019
ms.localizationpriority: high
ms.prod: windows-server-threshold
ms.openlocfilehash: 7af852ba934de2dd29a76972d7a7ec73606e9b4c
ms.sourcegitcommit: 4fa147d552481d8279a5390f458a9f7788061977
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2019
ms.locfileid: "70009118"
---
# <a name="windows-admin-center"></a>Windows Admin Center

> Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

**Windows Admin Center** (cuyo nombre en código era anteriormente **Project Honolulu**) es una evolución de las herramientas de administración incluidas de Windows Server; es un panel único que consolida todos los aspectos de la administración de servidores local y remota. Al tratarse de una experiencia de administración basada en explorador implementada localmente, no se requiere una conexión a Internet ni Azure. Windows Admin Center te ofrece control total de todos los aspectos de la implementación, incluidas redes privadas que no están conectadas a Internet.

## <a name="introduction"></a>Introducción

>[!VIDEO https://www.youtube.com/embed/PcQj6ZklmK0]

![Infografía de Windows Admin Center](media/WAC1809Poster_thumb.PNG)

[Descargar el PDF](https://github.com/MicrosoftDocs/windowsserverdocs/raw/master/WindowsServerDocs/manage/windows-admin-center/media/WindowsAdminCenter1809Poster.pdf)

## <a name="quick-start"></a>Inicio rápido

Puedes tener Windows Admin Center en funcionamiento y en ejecución en tu entorno en minutos:

1. [Descargar](https://aka.ms/windowsadmincenter)
2. [Instalar](deploy/install.md)
3. [Introducción](use/get-started.md)

## <a name="contents-at-a-glance"></a>Breve explicación del contenido

<table>
    <tr></tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Descripción</h3>
            <ul>
            <li><a href="understand/what-is.md">¿Qué es Windows Admin Center?</a>
            <li><a href="understand/faq.md">Preguntas más frecuentes</a>
            <li><a href="understand/case-studies.md">Casos prácticos</a>
            <li><a href="understand/related-management.md">Productos de administración relacionados</a>
            <li><a href="understand/videos.md">Vídeos</a>
            </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>Planear</h3>
            <ul>
            <li><a href="plan/installation-options.md">¿Qué tipo de instalación es el adecuado para usted?</a>
            <li><a href="plan/user-access-options.md">Opciones de acceso de usuario</a>
            <br>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Implementar</h3>
            <ul>
            <li><a href="deploy/prepare-environment.md">Preparación del entorno</a>
            <li><a href="deploy/install.md">Instalación de Windows Admin Center</a>
            <li><a href="deploy/high-availability.md">Habilitar la alta disponibilidad</a>
         </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>Configurar</h3>
            <ul>
            <li><a href="configure/settings.md">Configuración de Windows Admin Center</a>
            <li><a href="configure/user-access-control.md">Permisos y control de acceso de usuarios</a>
            <li><a href="configure/shared-connections.md">Conexiones compartidas</a>
            <li><a href="configure/using-extensions.md">Extensiones</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Use</h3>
            <ul>
            <li><a href="use/get-started.md">Iniciar y agregar conexiones</a>
            <li><a href="use/manage-servers.md">Administración de servidores</a>
            <li><a href="use/manage-hyper-converged.md">Administración de la infraestructura hiperconvergida</a>
            <li><a href="use/manage-failover-clusters.md">Administración de clústeres de conmutación por error</a>
            <li><a href="use/manage-virtual-machines.md">Administración de máquinas virtuales</a>
            <li><a href="use/logging.md">Inicio de sesión</a>
            </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>Conexión a Azure AD</h3>
            <ul>
            <li><a href="azure/index.md">Azure Hybrid Services</a></li>
            <li><a href="azure/azure-integration.md">Conexión de Windows Admin Center con Azure</a></li>
            <li><a href="azure/deploy-wac-in-azure.md">Implementación de Windows Admin Center en Azure</a></li>
            <li><a href="azure/manage-azure-vms.md">Administrar las VM de Azure con Windows Admin Center</a></li>
            </ul>
        </td>
    </tr>
    <tr>
            <td style="vertical-align: top;">
            <h3>Soporte</h3>
            <ul>
            <li><a href="support/index.md">Directiva de soporte</a>
            <li><a href="support/troubleshooting.md">Pasos de solución de problemas comunes</a>
            <li><a href="support/known-issues.md">Problemas conocidos</a>
            </ul>
        </td>
            <td style="vertical-align: top;">
            <h3>Extend</h3>
            <ul>
            <li><a href="extend/extensibility-overview.md">Información general de extensiones</a>
            <li><a href="extend/understand-extensions.md">Descripción de extensiones</a>
            <li><a href="extend/developing-extensions.md">Desarrollo de una extensión</a>
            <li><a href="extend/publish-extensions.md">Guías</a>
            <li><a href="extend/publish-extensions.md">Publicación de extensiones</a>
            </ul>
        </td>
    </tr>

</table>

## <a name="release-history"></a>Historial de versiones

Obtén información sobre las últimas funciones publicadas:

- Versión [1908](https://aka.ms/wac1908): incluye actualizaciones visuales, Packetmon, FlowLog Audit, incorporación de Azure Monitor para clústeres y compatibilidad con WinRM a través de HTTPS (puerto 5986).
- Versión [1907](https://aka.ms/wac1907): se han agregado vínculos de estimación de costos de Azure y se han realizado mejoras en la importación, exportación y etiquetado de máquinas virtuales.
- Versión [1906](https://aka.ms/wac1906): se han agregado máquinas virtuales de importación y exportación, cambiado las cuentas de Azure, agregado conexiones de Azure, el experimento de configuración de la conectividad, las mejoras de rendimiento y las herramientas de generación de perfiles.
- La versión 1904.1 es la versión más reciente con disponibilidad general: una actualización de mantenimiento para mejorar la estabilidad de los complementos de puerta de enlace.
- La versión [1904](https://aka.ms/wac1904) era una versión con disponibilidad general que introdujo la herramienta Azure Hybrid Services y que trajo las características que anteriormente se encontraban en la versión preliminar al canal de disponibilidad general.
- En la versión [1903](https://aka.ms/wac1903) se agregaron notificaciones por correo electrónico desde Azure Monitor, la posibilidad de agregar conexiones de servidor o de PC desde Active Directory y nuevas herramientas para administrar Active Directory, DHCP y DNS.
- En la versión [1902](https://aka.ms/wac1902) se agregó una lista de conexiones compartidas y mejoras en la administración de redes definidas por software entre las que se incluyen nuevas herramientas SDN para administrar ACL, conexiones de puerta de enlace y redes lógicas.
- En la versión [1812](https://aka.ms/wac1812) se agregó un tema oscuro (en versión preliminar), opciones de configuración de energía, información BMC y compatibilidad con PowerShell para administrar [extensiones](./configure/using-extensions.md#manage-extensions-with-powershell) y [conexiones](./use/get-started.md#use-powershell-to-import-or-export-your-connections-with-tags).
- La versión [1809.5](https://aka.ms/wac1809.5) era una actualización acumulativa de disponibilidad general que incluye una serie de mejoras de calidad y funcionales, así como correcciones de errores en toda la plataforma y algunas nuevas funciones en la solución de administración de la infraestructura hiperconvergida.
- La versión [1809](https://cloudblogs.microsoft.com/windowsserver/2018/09/20/windows-admin-center-1809-and-sdk-now-generally-available/) era una versión de disponibilidad general que aportó funciones que anteriormente estaban en versión preliminar en el canal de disponibilidad general.
- En la versión [1808](https://aka.ms/WACPreview1808-InsiderBlog) se agregó la herramienta Aplicaciones instaladas, una gran cantidad de mejoras de fondo y actualizaciones importantes del SDK en versión preliminar.
- En la versión [1807](https://aka.ms/WACPreview1807-InsiderBlog) se agregó una experiencia de conexión optimizada de Azure, mejoras en la página de inventario de VM, funcionalidad de uso compartido de archivos, integración de administración de actualizaciones en Azure y mucho más. 
- En la versión [1806](https://aka.ms/WACPreview1806-InsiderBlog) se agregó la posibilidad de mostrar el script de PowerShell, administración SDN, conexiones de 2008 R2, SDN, tareas programadas y muchas otras mejoras.
- Versión 1804.25: una actualización de mantenimiento para dar soporte técnico a usuarios que instalaban Windows Admin Center en entornos completamente sin conexión.
- Versión [1804](https://cloudblogs.microsoft.com/windowsserver/2018/04/12/announcing-windows-admin-center-our-reimagined-management-experience/): el Proyecto Honolulu se convierte en Windows Admin Center y permite agregar características de seguridad y control de acceso basado en rol. Nuestra primera versión de disponibilidad general.
- En la versión [1803](https://blogs.windows.com/windowsexperience/2018/03/13/announcing-project-honolulu-technical-preview-1803-and-rsat-insider-preview-for-windows-10) se agregó compatibilidad con el control de acceso de Azure AD, registro detallado, reajuste de tamaño de contenido y gran número de mejoras de herramientas.
- En la versión [1802](https://blogs.windows.com/windowsexperience/2018/02/13/announcing-windows-server-insider-preview-build-17093-project-honolulu-technical-preview-1802) se agregó compatibilidad para accesibilidad, localización, implementaciones de alta disponibilidad, etiquetado, configuración del host de Hyper-V y autenticación de puerta de enlace.
- En la versión [1712](https://blogs.windows.com/windowsexperience/2017/12/19/announcing-project-honolulu-technical-preview-1712-build-05002) se agregaron más características de máquina virtual y mejoras de rendimiento en las herramientas.
- En la versión [1711](https://cloudblogs.microsoft.com/windowsserver/2017/12/01/1711-update-to-project-honolulu-technical-preview-is-now-available/) se agregaron herramientas ya previstas desde hacía mucho tiempo (Escritorio remoto y PowerShell) junto con otras mejoras.
- La versión [1709](https://cloudblogs.microsoft.com/windowsserver/2017/09/22/project-honolulu-technical-preview-is-now-available-for-download/) se lanzó como nuestra primera versión preliminar pública.

## <a name="stay-updated"></a>Mantente al día

![ ](//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOolR)[Síguenos en Twitter](https://twitter.com/servermgmt)

![ ](//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOtyw)[Lee nuestros blogs](https://blogs.technet.microsoft.com/servermanagement/)

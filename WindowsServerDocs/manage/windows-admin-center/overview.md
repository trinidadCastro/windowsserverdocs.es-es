---
title: Información general de Windows Admin Center
description: Obtén información sobre cómo administrar Windows Server con Windows Admin Center (Proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/07/2019
ms.localizationpriority: high
ms.prod: windows-server-threshold
ms.openlocfilehash: 2314e336cbf9ad44b07f3f94d7a866b48b5e9bff
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811810"
---
# <a name="windows-admin-center"></a>Windows Admin Center

> Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

**Windows Admin Center** (anteriormente con el código **proyecto Honolulu**) es una evolución de Windows Server en el cuadro de herramientas de administración; es un único panel de vidrio que consolida todos los aspectos de administración de servidores locales y remotos. Al tratarse de una experiencia de administración basada en explorador implementada localmente, no se requiere una conexión a Internet ni Azure. Windows Admin Center te ofrece control total de todos los aspectos de la implementación, incluidas redes privadas que no están conectadas a Internet.

## <a name="introduction"></a>Introducción

>[!VIDEO https://www.youtube.com/embed/PcQj6ZklmK0]

![Infografía de Windows Admin Center](media/WAC1809Poster_thumb.PNG)

[Descargue el archivo .PDF](https://github.com/MicrosoftDocs/windowsserverdocs/raw/master/WindowsServerDocs/manage/windows-admin-center/media/WindowsAdminCenter1809Poster.pdf)

## <a name="quick-start"></a>Inicio rápido

Puedes tener Windows Admin Center en funcionamiento y ejecutándose en tu entorno en minutos:

1. [Descargar](https://aka.ms/windowsadmincenter)
2. [Instalar](deploy/install.md)
3. [Introducción](use/get-started.md)

## <a name="contents-at-a-glance"></a>Contenido de un vistazo

<table>
    <tr></tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Comprender</h3>
            <ul>
            <li><a href="understand/what-is.md">¿Qué es Windows Admin Center?</a>
            <li><a href="understand/faq.md">PREGUNTAS MÁS FRECUENTES</a>
            <li><a href="understand/case-studies.md">Casos prácticos</a>
            <li><a href="understand/related-management.md">Productos de administración relacionados</a>
            <li><a href="understand/videos.md">Vídeos</a>
            </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>Planear</h3>
            <ul>
            <li><a href="plan/installation-options.md">¿Qué tipo de instalación es adecuado para usted?</a>
            <li><a href="plan/user-access-options.md">Opciones de acceso de usuario</a>
            <br>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Implementar</h3>
            <ul>
            <li><a href="deploy/prepare-environment.md">Preparar el entorno</a>
            <li><a href="deploy/install.md">Instalar Windows Admin Center</a>
            <li><a href="deploy/high-availability.md">Habilitar la alta disponibilidad</a>
         </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>Configurar</h3>
            <ul>
            <li><a href="configure/settings.md">Configuración de Windows Admin Center</a>
            <li><a href="configure/user-access-control.md">Control de acceso de usuario y permisos</a>
            <li><a href="configure/shared-connections.md">Conexiones compartidas</a>
            <li><a href="configure/using-extensions.md">Extensiones</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Usar</h3>
            <ul>
            <li><a href="use/get-started.md">Iniciar & Agregar conexiones</a>
            <li><a href="use/manage-servers.md">Administrar servidores</a>
            <li><a href="use/manage-hyper-converged.md">Administrar infraestructuras hiperconvergidas</a>
            <li><a href="use/manage-failover-clusters.md">Administración de clústeres de conmutación por error</a>
            <li><a href="use/manage-virtual-machines.md">Administración de máquinas virtuales</a>
            <li><a href="use/logging.md">Registro</a>
            </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>Conexión a Azure AD</h3>
            <ul>
            <li><a href="azure/index.md">Servicios híbridos de Azure</a></li>
            <li><a href="azure/azure-integration.md">Conectar Windows Admin Center en Azure</a></li>
            <li><a href="azure/deploy-wac-in-azure.md">Implementar Windows Admin Center en Azure</a></li>
            <li><a href="azure/manage-azure-vms.md">Administrar las VM de Azure con Windows Admin Center</a></li>
            </ul>
        </td>
    </tr>
    <tr>
            <td style="vertical-align: top;">
            <h3>Soporte</h3>
            <ul>
            <li><a href="support/index.md">Directiva de soporte técnico</a>
            <li><a href="support/troubleshooting.md">Pasos para solucionar problemas comunes</a>
            <li><a href="support/known-issues.md">Problemas conocidos</a>
            </ul>
        </td>
            <td style="vertical-align: top;">
            <h3>Extend</h3>
            <ul>
            <li><a href="extend/extensibility-overview.md">Información general de extensiones</a>
            <li><a href="extend/understand-extensions.md">Extensiones de descripción</a>
            <li><a href="extend/developing-extensions.md">Desarrollar una extensión</a>
            <li><a href="extend/publish-extensions.md">Guías</a>
            <li><a href="extend/publish-extensions.md">Publicación de extensiones</a>
            </ul>
        </td>
    </tr>

</table>

## <a name="release-history"></a>Historial de versiones

Obtén información sobre las últimas funciones publicadas:

- Versión 1904.1 - actualización de mantenimiento para mejorar la estabilidad de los complementos de puerta de enlace.
- Versión [1904](https://aka.ms/wac1904) es la versión más reciente de GA que presenta la herramienta servicios híbridos de Azure y ofrece características que anteriormente se encontraban en versión preliminar para el canal de GA.
- Versión [1903](https://aka.ms/wac1903) ofrece notificaciones de correo electrónico de Azure Monitor, la capacidad para agregar conexiones de servidor o equipo de Active Directory y las nuevas herramientas para administrar Active Directory, DHCP y DNS.
- Versión [1902](https://aka.ms/wac1902) agrega una lista de conexiones compartidas y mejoras a la administración de redes definidas por software, incluidas nuevas herramientas SDN para administrar las ACL, las conexiones de puerta de enlace y redes lógicas.
- La versión [1812](https://aka.ms/wac1812) agregó un tema oscuro (en versión preliminar), opciones de configuración de energía, información BMC y compatibilidad con PowerShell para administrar [las extensiones](./configure/using-extensions.md#manage-extensions-with-powershell) y [las conexiones](./use/get-started.md#use-powershell-to-import-or-export-your-connections-with-tags).
- La versión [1809.5](https://aka.ms/wac1809.5) es una actualización acumulativa de disponibilidad general que incluye una serie de mejoras de calidad y funcionales, así como correcciones de errores en toda la plataforma y algunas nuevas funciones en la solución de administración de la infraestructura hiperconvergida.
- La versión [1809](https://cloudblogs.microsoft.com/windowsserver/2018/09/20/windows-admin-center-1809-and-sdk-now-generally-available/) era una versión de disponibilidad general que aportó funciones que anteriormente estaban en la vista previa en el canal de disponibilidad general.
- La versión [1808](https://aka.ms/WACPreview1808-InsiderBlog) agregó la herramienta Aplicaciones instaladas, una gran cantidad de mejoras de fondo y actualizaciones importantes del SDK de vista previa.
- La versión [1807](https://aka.ms/WACPreview1807-InsiderBlog) agregó una experiencia de conexión optimizada de Azure, mejoras en la página de inventario de VM, funcionalidad de uso compartido de archivos, integración de administración de actualizaciones en Azure y mucho más. 
- La versión [1806](https://aka.ms/WACPreview1806-InsiderBlog) agregó mostrar el script de PowerShell, administración SDN, conexiones de 2008 R2, SDN, tareas programadas y muchas otras mejoras.
- Versión 1804.25 - Actualización de mantenimiento para admitir usuarios instalando Windows Admin Center en entornos completamente sin conexión.
- Versión [1804](https://cloudblogs.microsoft.com/windowsserver/2018/04/12/announcing-windows-admin-center-our-reimagined-management-experience/) - El Proyecto Honolulu se convierte en Windows Admin Center y agrega características de seguridad y control de acceso basado en roles. Nuestra primera versión de disponibilidad general.
- La versión [1803](https://blogs.windows.com/windowsexperience/2018/03/13/announcing-project-honolulu-technical-preview-1803-and-rsat-insider-preview-for-windows-10) agregó compatibilidad para control de acceso de Azure AD, registro detallado, reajuste de tamaño de contenido y un montón de mejoras de herramientas.
- La versión [1802](https://blogs.windows.com/windowsexperience/2018/02/13/announcing-windows-server-insider-preview-build-17093-project-honolulu-technical-preview-1802) agregó compatibilidad para accesibilidad, localización, implementaciones de alta disponibilidad, etiquetado, configuración del host de Hyper-V y autenticación de puerta de enlace.
- La versión [1712](https://blogs.windows.com/windowsexperience/2017/12/19/announcing-project-honolulu-technical-preview-1712-build-05002) agregó más características de máquina virtual y mejoras de rendimiento en las herramientas.
- La versión [1711](https://cloudblogs.microsoft.com/windowsserver/2017/12/01/1711-update-to-project-honolulu-technical-preview-is-now-available/) agregó herramientas altamente anticipadas (Escritorio remoto y PowerShell) junto con otras mejoras.
- La versión [1709](https://cloudblogs.microsoft.com/windowsserver/2017/09/22/project-honolulu-technical-preview-is-now-available-for-download/) se lanzó como nuestra primera versión preliminar pública.

## <a name="stay-updated"></a>Manténgase al día

![ ](//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOolR)[Síganos en Twitter](https://twitter.com/servermgmt)

![ ](//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOtyw)[Lea nuestra Blogs](https://blogs.technet.microsoft.com/servermanagement/)
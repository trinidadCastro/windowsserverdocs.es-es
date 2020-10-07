---
title: Introducción a Windows Admin Center
description: Aprenda a administrar Windows Server con Windows Admin Center (Proyecto Honolulu)
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 01/07/2020
ms.localizationpriority: high
ms.openlocfilehash: a62994667322b17d35c90e8bd231adc1d9ba0e6a
ms.sourcegitcommit: f89639d3861c61620275c69f31f4b02fd48327ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2020
ms.locfileid: "91517571"
---
# <a name="windows-admin-center"></a>Windows Admin Center

> Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

Windows Admin Center es una aplicación basada en explorador e implementada localmente para la administración de servidores Windows, clústeres, infraestructuras hiperconvergidas y PC Windows 10. Es un producto gratuito y está listo para usarse en producción.

Para descubrir las novedades, consulta el [historial de versiones](support/release-history.md).

## <a name="download-now"></a>Descargar ahora

**Descarga [Windows Admin Center](https://www.microsoft.com/evalcenter/evaluate-windows-admin-center)** desde Microsoft Evaluation Center. Aunque ponga "Iniciar la evaluación", esta es la versión disponible con carácter general para su uso en producción.

Para obtener ayuda con la instalación, consulta [Instalar](deploy/install.md). Para obtener sugerencias sobre cómo empezar a trabajar con Windows Admin Center, consulta [Introducción](use/get-started.md).

Puedes actualizar versiones no preliminares de Windows Admin Center mediante Microsoft Update o mediante la descarga e instalación manuales de Windows Admin Center. Cada versión de Windows Admin Center que no es la versión preliminar dispone de soporte técnico hasta 30 días después del lanzamiento de la siguiente versión que no sea preliminar. Consulte nuestra [directiva de soporte técnico](support/index.md) para más información.

## <a name="windows-admin-center-scenarios"></a>Escenarios de Windows Admin Center

Estas son algunas de las cosas que puedes hacer con Windows Admin Center:

- **Simplificar la gestión de los servidores** Administre sus servidores y clústeres con versiones modernizadas de herramientas tan conocidas como el Administrador del servidor. Instálalo en menos de 5 minutos y administra los servidores de tu entorno inmediatamente: no se requiere ninguna configuración adicional. Para obtener más detalles, consulta [¿Qué es Windows Admin Center?](understand/what-is.md)

- **Trabajar con soluciones híbridas** La integración con Azure le ayuda a conectar de forma opcional sus servidores locales con los servicios en la nube pertinentes. Para obtener más información, consulta [Servicios híbridos de Azure](azure/index.md)

- **Simplificar la administración hiperconvergida** Simplifique la administración de los clústeres hiperconvergidos de Windows Server o Azure Stack HCI. Usa cargas de trabajo simplificadas para crear y administrar VM, volúmenes de Espacios de almacenamiento directo, redes definidas por software y mucho más. Para obtener más detalles, consulta [Administrar la infraestructura hiperconvergida con Windows Admin Center](use/manage-hyper-converged.md)

A continuación se incluye un vídeo a modo de introducción, seguido de un póster con más detalles:
> [!VIDEO https://www.youtube.com/embed/WCWxAp27ERk]

[![Póster de Windows Admin Center](media/WAC1910Poster_thumb_small.PNG)](media/WAC1910Poster_thumb.png)

[Descargar el PDF](https://github.com/MicrosoftDocs/windowsserverdocs/raw/master/WindowsServerDocs/manage/windows-admin-center/media/WindowsAdminCenter1910Poster.pdf)

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
            <li><a href="configure/use-powershell.md">Automatizar con PowerShell</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Use</h3>
            <ul>
            <li><a href="use/get-started.md">Iniciar y agregar conexiones</a>
            <li><a href="use/manage-servers.md">Administración de servidores</a>
            <li><a href="use/deploy-hyperconverged-infrastructure.md">Implementar la infraestructura hiperconvergida</a>
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
            <h3>Soporte técnico</h3>
            <ul>
            <li><a href="support/release-history.md">Historial de versiones</a>
            <li><a href="support/index.md">Directiva de soporte</a>
            <li><a href="support/troubleshooting.md">Pasos de solución de problemas comunes</a>
            <li><a href="support/known-issues.md">Problemas conocidos</a>
            </ul>
        </td>
            <td style="vertical-align: top;">
            <h3>Extensión</h3>
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

## <a name="video-based-learning"></a>Aprendizaje basado en vídeo

A continuación se incluyen algunos vídeos de la sesiones de Microsoft Ignite 2019:

- [Windows Admin Center: liberar el valor de los escenarios híbridos de Azure](https://aka.ms/WAC-BRK3165)
- [Windows Admin Center: novedades y características futuras](https://aka.ms/WAC-BRK2048)
- [Supervisión, protección y actualización automáticas de los servidores locales de Azure con Windows Admin Center](https://aka.ms/WAC-THR2146)
- [Hacer más cosas con las extensiones de terceros de Windows Admin Center](https://aka.ms/WAC-THR2140)
- [Cómo convertirse en experto en Windows Admin Center: prácticas recomendadas de implementación, configuración y seguridad](https://aka.ms/WAC-THR2135)
- [Windows Admin Center: mejor junto con System Center y Microsoft Azure](https://aka.ms/WAC-THR2176)
- [Cómo usar los servicios híbridos de Microsoft Azure junto con Windows Admin Center y Windows Server](https://aka.ms/WAC-THR2073)
- [Preguntas y respuestas en directo: administrar el entorno de servidor híbrido con Windows Admin Center](https://aka.ms/WAC-MLS1055)
- [Ruta de aprendizaje: tecnologías híbridas de administración](https://aka.ms/WAC-HybridMgmtTech)
- [Laboratorio práctico: Windows Admin Center y escenarios híbridos](/learn/?WT.mc_id=sitertzn_homepage_learn-redirect-handsonlabs)

Estos son algunos vídeos de las sesiones de Windows Server Summit 2019:

- [Empezar a trabajar en entornos híbridos con Windows Admin Center](https://aka.ms/WAC-WSS2019-GoHybridWAC)
- [Novedades de Windows Admin Center v1904](https://aka.ms/WAC-WSS2019-WhatsNewv1904)

A continuación se incluyen algunos recursos adicionales:

- [Redefinición de la administración de servidores con Windows Admin Center](https://aka.ms/WAC-ServerMgmtReimagined)
- [Administrar servidores y máquinas virtuales desde cualquier lugar con Windows Admin Center](https://aka.ms/WAC-Webinar2019)
- [Cómo empezar a trabajar con Windows Admin Center](https://www.youtube.com/embed/PcQj6ZklmK0)

## <a name="see-how-customers-are-benefitting-from-windows-admin-center"></a>Descubre cómo los clientes se benefician de Windows Admin Center

|     |
| --- |
| "[Windows Admin Center] ha disminuido nuestro tiempo y esfuerzo a la hora de administrar el sistema de administración en más de un 75 %".<br> *- Rand Morimoto, Presidente, Convergent Computing* |
| "Gracias a [Windows Admin Center], podemos administrar nuestros clientes de forma remota desde el portal de HTML5 sin problema y con la integración completa con Azure Active Directory, podemos aumentar la seguridad gracias a Multi-Factor Authentication".<br/> *- Silvio Di Benedetto, Fundador y Consultor ejecutivo, Inside Technologies* |
| "Hemos logrado implementar los SKU de [Server Core] de una manera más eficaz, mejorar la eficiencia de los recursos, la seguridad y la automatización mientras se consigue un buen nivel de productividad y se reducen los errores que pueden ocurrir cuando todo depende de scripts". <br/> *- Guglielmo Mengora, Fundador y Director general, VaiSulWeb* |
| "Con [Windows Admin Center] los clientes especialmente en el mercado SMB ahora tienen una herramienta fácil de usar para administrar su infraestructura interna. Esto minimiza los esfuerzos administrativos y ahorra una gran cantidad de tiempo. Y lo mejor de todo: no se aplica ninguna tasa de licencia adicional para [Windows Admin Center]". <br/> *- Helmut Otto, Director Ejecutivo, SecureGUARD* |

[Obtén más información acerca de las empresas que utilizan Windows Admin Center en sus entornos de producción.](understand/case-studies.md)

## <a name="related-products"></a>Productos relacionados

Windows Admin Center está diseñado para administrar un solo servidor o clúster. Complementa pero no sustituye a las soluciones de supervisión y administración de Microsoft existentes, como las Herramientas de administración remota del servidor (RSAT), System Center, Intune o Azure Stack.

[Obtén información sobre cómo Windows Admin Center complementa otras soluciones de administración de Microsoft](understand/related-management.md).

## <a name="stay-updated"></a>Mantente al día

[Síguenos en Twitter](https://twitter.com/servermgmt)

[Lea nuestros blogs](https://techcommunity.microsoft.com/t5/windows-admin-center-blog/bg-p/Windows-Admin-Center-Blog)
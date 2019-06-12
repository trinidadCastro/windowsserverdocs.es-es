---
title: Preguntas más frecuentes acerca de Windows Admin Center
description: Obtén respuestas sobre Windows Admin Center (Proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.openlocfilehash: 5c306dd181d4db400e6ab5bab919399fdebca9f3
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811666"
---
# <a name="windows-admin-center-frequently-asked-questions"></a>Preguntas más frecuentes acerca de Windows Admin Center

> Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

Aquí encontrarás respuestas a las preguntas más frecuentes acerca de Windows Admin Center.

## <a name="what-is-windows-admin-center"></a>¿Qué es Windows Admin Center?

Windows Admin Center es una plataforma de GUI basada en explorador y un conjunto de herramientas para los administradores de TI para administrar Windows Server y Windows 10. Es la evolución de herramientas administrativas predeterminadas, como Administrador de servidores y Microsoft Management Console (MMC) en una experiencia moderna, simplificada, integrada y segura.

## <a name="can-i-use-windows-admin-center-in-production-environments"></a>¿Puedo usar Windows Admin Center en entornos de producción?

Sí. Windows Admin Center generalmente está disponible y listo para implementaciones amplias de uso y de producción. Las capacidades actuales de la plataforma y herramientas básicas cumplen los criterios de versión estándar de Microsoft y nuestra barra de calidad para la adopción, confiabilidad, rendimiento, accesibilidad, seguridad y facilidad de uso.

[!INCLUDE [support-policy](../includes/support-policy.md)]

## <a name="how-much-does-it-cost-to-use-windows-admin-center"></a>¿Cuánto cuesta usar Windows Admin Center?

Windows Admin Center no tiene ningún coste adicional más allá de Windows. Puedes usar Windows Admin Center (disponible como una descarga independiente) con licencias válidas de Windows Server o Windows 10 sin coste adicional; se concede bajo licencia en un EULA complementario de Windows.

## <a name="what-versions-of-windows-server-can-i-manage-with-windows-admin-center"></a>¿Qué versiones de Windows Server puedo administrar con Windows Admin Center?

Windows Admin Center está optimizado para que Windows Server 2019 temas clave en la versión de Windows Server 2019: híbridas y en la nube escenarios de administración de infraestructuras hiperconvergidas en particular. Aunque Windows Admin Center funcionará mejor con Windows Server 2019, admite la administración de una variedad de versiones que ya usan los clientes: Windows Server 2012 y versiones posterior son totalmente compatibles. También es una funcionalidad limitada para administrar Windows Server 2008 R2.

## <a name="is-windows-admin-center-a-complete-replacement-for-all-traditional-in-box-and-rsat-tools"></a>¿Es Windows Admin Center un reemplazo completo para todos las herramientas RSAT y predeterminadas tradicionales?

No. Aunque Windows Admin Center puede administrar muchos escenarios comunes, no reemplaza completamente todas las herramientas tradicionales de Microsoft Management Console (MMC). Para una visión detallada de qué herramientas se incluyen con Windows Admin Center, obtenga más información sobre [administrar servidores](../use/manage-servers.md) en nuestra documentación. Windows Admin Center tiene las siguientes capacidades claves de su solución de Administrador de servidores:

* Visualización y utilización de recursos
* Administración de certificados
* Administración de dispositivos
* Visor de eventos
* Explorador de archivos
* Administración de firewalls
* Administrar las aplicaciones instaladas
* Configuración de grupos y usuarios locales
* Configuración de red
* Visualización y finalización de procesos y creación de volcados de proceso
* Edición del registro
* Administración de tareas programadas
* Administración de servicios de Windows
* Habilitación y deshabilitación de roles y características
* Administración de VM Hyper-V y conmutadores virtuales
* Administración de almacenamiento
* Administración de réplica de almacenamiento
* Administración de actualizaciones de Windows
* Consola de PowerShell
* Conexión a Escritorio remoto

Windows Admin Center también proporciona estas soluciones:

* Administración de equipos: proporciona un subconjunto de características del Administrador de servidores para la administración de equipos cliente de Windows 10
* Administrador de clústeres de conmutación por error: proporciona compatibilidad para la administración continua de clústeres de conmutación por error y recursos de clúster
* Administrador de clústeres hiperconvergidos:proporciona una experiencia completamente nueva adaptada para espacios de almacenamiento Direct y Hyper-V. Presenta el Panel y enfatiza gráficos y alertas para la supervisión.

Windows Admin Center es complementario y no sustituye a RSAT (herramientas de administración de servidor remoto) ya que roles como Active Directory, DHCP, DNS, IIS todavía no tienen capacidades de administración equivalentes expuestas en Windows Admin Center.

## <a name="can-windows-admin-center-be-used-to-manage-the-free-microsoft-hyper-v-server"></a>¿Puede utilizarse Windows Admin Center para administrar el servidor gratuito de Microsoft Hyper-V?

Sí. Windows Admin Center puede usarse para administrar Microsoft Hyper-V Server 2016 y Microsoft Hyper-V Server 2012 R2.

## <a name="can-i-deploy-windows-admin-center-on-a-windows-10-computer"></a>¿Puedo implementar Windows Admin Center en un equipo Windows 10?

Sí, Windows Admin Center se puede instalar en Windows 10 (versión 1709 o posterior), ejecutando en modo de escritorio.  Windows Admin Center también se puede instalar en un servidor con Windows Server 2016 o superior en el modo de puerta de enlace y después se accede a través de un explorador web desde un equipo Windows 10. [Obtén más información acerca de las opciones de instalación](../plan/installation-options.md).

## <a name="ive-heard-that-windows-admin-center-uses-powershell-under-the-hood-can-i-see-the-actual-scripts-that-it-uses"></a>He oído que Windows Admin Center usa PowerShell debajo del capó, ¿puedo ver las secuencias de comandos reales que utiliza?

¡Sí! el [Showscript característica](../use/get-started.md#view-powershell-scripts-used-in-windows-admin-center) se agregó en Windows Admin Center Preview 1806 y ahora se incluye en el canal de GA.

## <a name="are-there-any-plans-for-windows-admin-center-to-manage-windows-server-2008-r2-or-earlier"></a>¿Existen planes para que Windows Admin Center administre Windows Server 2008 R2 o una versión anterior?

Windows Admin Center ahora admite **limitado** funcionalidad para administrar Windows Server 2008 R2. Windows Admin Center se basa en funciones de PowerShell y las tecnologías de plataforma que no existen en Windows Server 2008 R2 y versiones anteriores, lo que hace que el soporte completo no sea factible. Windows Server 2008/2008 R2 se aproximan a fin de soporte en enero de 2020 por lo que Microsoft recomienda a los clientes [movimiento a Azure o la actualización a la versión más reciente de Windows Server](https://www.microsoft.com/en-us/cloud-platform/windows-server-2008).

## <a name="are-there-any-plans-for-windows-admin-center-to-manage-linux-connections"></a>¿Existen planes para que Windows Admin Center administre conexiones de Linux?

Estamos investigando debido a la demanda del cliente, pero actualmente no hay ningún plan bloqueado para entregar y soporte técnico puede constar solo de una conexión de consola a través de SSH.

## <a name="which-web-browsers-are-supported-by-windows-admin-center"></a>¿Qué exploradores web son compatibles con Windows Admin Center?

Las versiones más recientes de Microsoft Edge (Windows 10, versión 1709 o posterior) y los exploradores Google Chrome están probadas y se admiten en Windows 10. [Problemas conocidos específico de explorador de vista](../support/known-issues.md#browser-specific-issues). Otros exploradores web modernos u otras plataformas, actualmente no forman parte de nuestra matriz de pruebas y no están *oficialmente* compatibles.

## <a name="how-does-windows-admin-center-handle-security"></a>¿Cómo controla Windows Admin Center la seguridad?

El tráfico desde el explorador a la puerta de enlace de Windows Admin Center usa HTTPS. El tráfico desde la puerta de enlace a los servidores administrados es PowerShell y WMI estándar a través de WinRM. Admitimos LAPS (Solución de contraseñas de administrador local), delegación limitada basada en recursos, control de acceso de puerta de enlace mediante AD o Azure AD, y control de acceso basado en roles para administrar servidores de destino.

## <a name="does-windows-admin-center-use-credssp"></a>¿Windows Admin Center usa CredSSP?

Sí, en algunos casos, Windows Admin Center requiere CredSSP. Esto es necesario para pasar las credenciales de autenticación a máquinas más allá del servidor específico tiene como destino para la administración. Por ejemplo, si está administrando máquinas virtuales en **servidor B**, pero desea almacenar los archivos vhdx para esas máquinas virtuales en un recurso compartido de archivos hospedado por **servidor C**, Windows Admin Center debe usar CredSSP para autenticar con **servidor C** para tener acceso a recurso compartido de archivos.

Windows Admin Center administra la configuración de CredSSP automáticamente después de solicitar el consentimiento del usuario. Antes de configurar CredSSP, Windows Admin Center comprobará para asegurarse de que el sistema tiene CredSSP reciente [actualizaciones](https://support.microsoft.com/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018). Mientras se habilita CredSSP, habrá un distintivo en la información general del servidor y una opción para deshabilitarlo:

![CredSSP en información general del servidor](../media/CredSSP-overview.png)

CredSSP se utiliza actualmente en las áreas siguientes:

- Uso de desagregada almacenamiento SMB en la herramienta de máquinas virtuales (el ejemplo anterior).
- Uso de las actualizaciones de herramientas en la conmutación por error o un clúster Hiperconvergido soluciones de administración, que lleva a cabo [Cluster-Aware Updating](https://docs.microsoft.com/windows-server/failover-clustering/cluster-aware-updating) 

## <a name="are-there-any-cloud-dependencies"></a>¿Hay alguna dependencia de la nube?

Windows Admin Center no requiere acceso a Internet y no requiere Microsoft Azure. Windows Admin Center administra Windows Server e instancias de Windows en cualquier lugar: en sistemas físicos o en máquinas virtuales en cualquier hipervisor o ejecutándose en cualquier nube. Aunque con el tiempo se agregará la integración con distintos servicios de Azure, estas serán características de valor añadido opcionales y no es un requisito para usar Windows Admin Center.

## <a name="are-there-any-other-dependencies-or-prerequisites"></a>¿Existen otras dependencias o requisitos previos?

Windows Admin Center se puede instalar en Windows 10 Fall Anniversary Update (1709) o versión más reciente, o Windows Server 2016 o versión más reciente. Para administrar Windows Server 2008 R2, 2012 o 2012 R2, se requiere la instalación de Windows Management Framework 5.1 en esos servidores. No existen otras dependencias. No se requiere IIS, no se requieren agentes, no se requiere SQL Server.

## <a name="what-about-extensibility-and-3rd-party-support"></a>¿Qué sucede con la extensibilidad y la compatibilidad con proveedores de terceros?

Windows Admin Center tiene un SDK disponible para que cualquier usuario puede escribir su propia extensión. Como plataforma, aumentar nuestro ecosistema y posibilitar la extensibilidad de partners ha sido una prioridad clave desde el principio. [Más información acerca del SDK de Windows Admin Center](../extend/extensibility-overview.md).

## <a name="can-i-manage-hyper-converged-infrastructure-with-windows-admin-center"></a>¿Puedo administrar la infraestructura hiperconvergida con Windows Admin Center?

Sí. Windows Admin Center admite la administración de clústeres hiperconvergido que ejecuta Windows Server 2016 o Windows Server 2019. La solución de clúster hiperconvergido manager en Windows Admin Center que estaba en versión preliminar, pero ahora es **disponible con carácter general**, con algunas funcionalidades nuevas en vista previa. Para obtener más información, [lee más acerca de la administración de la infraestructura hiperconvergida](../use/manage-hyper-converged.md).

## <a name="does-windows-admin-center-require-system-center"></a>¿Necesita Windows Admin Center System Center?

No. Windows Admin Center es complementario para System Center, pero System Center no es necesario. [Obtenga más información acerca de Windows Admin Center y System Center](related-management.md#system-center).

## <a name="can-windows-admin-center-replace-system-center-virtual-machine-manager-scvmm"></a>¿Puede reemplazar Windows Admin Center a System Center Virtual Machine Manager (SCVMM)?

Windows Admin Center y SCVMM son complementarias; Windows Admin Center está pensado para reemplazar los complementos tradicionales de Microsoft Management Console (MMC) y la experiencia de administración de servidores.  Windows Admin Center no está pensado para sustituir los aspectos de supervisión de SCVMM. [Obtenga más información acerca de Windows Admin Center y System Center](related-management.md#system-center).

## <a name="what-is-windows-admin-center-preview-which-version-is-right-for-me"></a>¿Qué es la versión preliminar de Windows Admin Center, qué versión es la adecuada para mí?

Hay dos versiones de Windows Admin Center disponibles para su descarga:

### <a name="windows-admin-center"></a>Windows Admin Center

* Esta versión es para administradores de TI que no pueden actualizar con frecuencia o que desean más tiempo de validación para las versiones que se utilizan en producción. Nuestra versión (GA) disponible con carácter general actual es Windows Admin Center 1904.
* [!INCLUDE [support-policy](../includes/support-policy.md)]
* Para obtener la versión más reciente, [descargar aquí](https://aka.ms/WACDownload).

### <a name="windows-admin-center-preview"></a>Versión preliminar de Windows Admin Center

> [!NOTE]
> La versión actual de la disponibilidad general (Windows Admin Center 1904) contiene toda la funcionalidad de vista previa anterior.
> La versión preliminar de Insider devolverá en los próximos meses.

* Esta versión es para administradores de TI que desean las características mejores y más recientes en una cadencia regular. Nuestra intención es proporcionar la posterior actualización libera todos los meses aproximadamente. La plataforma principal sigue estando lista para la producción y la licencia proporciona derechos de uso de producción. Sin embargo, ten en cuenta que podrás ver la introducción de nuevas herramientas y funciones que están marcadas claramente como VERSIÓN PRELIMINAR y son adecuadas para evaluar y probar.
* Para obtener la última versión preliminar de Insider, Insider registrado puede descargar la versión preliminar de Windows Admin Center directamente desde el [página de descarga de Windows Server Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver), en la lista desplegable descargas adicionales. Si aún no te has registrado como usuario de Insider, consulta la [Introducción a Windows Server](https://insider.windows.com/en-us/for-business-getting-started-server/) en el portal de Windows Insider para empresas.

## <a name="why-was-windows-admin-center-chosen-as-the-final-name-for-project-honolulu"></a>¿Por qué se ha seleccionado "Windows Admin Center" como el nombre final para el "Proyecto Honolulu"?

Windows Admin Center es el nombre oficial del producto del "Proyecto Honolulu" y refuerza nuestra visión de una experiencia integrada para los administradores de TI a través de una gran variedad de escenarios administrativos y de gestión. También destaca nuestro enfoque centrado en el cliente en las necesidades de los usuarios de administración de TI como fundamentales para la forma en que invertimos y lo que ofrecemos.

## <a name="where-can-i-learn-more-about-windows-admin-center-or-get-more-details-on-the-topics-above"></a>¿Dónde puedo obtener más información Windows Admin Center u obtener información más detallada sobre los temas anteriores?

Nuestra [página de inicio](https://aka.ms/WindowsAdminCenter) es el mejor punto de partida e incluye vínculos al contenido recientemente clasificado de nuestra documentación, la ubicación de descarga, cómo proporcionar comentarios, información de referencia y otros recursos.

## <a name="what-is-the-version-history-of-windows-admin-center"></a>¿Qué es el historial de versiones de Windows Admin Center?

[Ver el historial de versiones aquí.](../overview.md#release-history)

## <a name="im-having-an-issue-with-windows-admin-center-where-can-i-get-help"></a>Tengo un problema con Windows Admin Center, ¿dónde puedo obtener ayuda?

Consulta nuestra [Guía de solución de problemas](../use/troubleshooting.md) y nuestra lista de [problemas conocidos](../use/known-issues.md).

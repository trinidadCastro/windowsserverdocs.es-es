---
title: Preguntas más frecuentes acerca de Windows Admin Center
description: Obtén respuestas sobre Windows Admin Center (Proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 04/12/2019
ms.prod: windows-server-threshold
ms.openlocfilehash: 2f1591a32147e3c11ba635f1a11d36b7f38a3470
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296747"
---
# Preguntas más frecuentes acerca de Windows Admin Center

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Aquí encontrarás respuestas a las preguntas más frecuentes acerca de Windows Admin Center.

## ¿Qué es Windows Admin Center?

Windows Admin Center es una plataforma de GUI basada en explorador y un conjunto de herramientas para los administradores de TI para administrar Windows Server y Windows 10. Es la evolución de herramientas administrativas predeterminadas, como Administrador de servidores y Microsoft Management Console (MMC) en una experiencia moderna, simplificada, integrada y segura.

## ¿Puedo usar Windows Admin Center en entornos de producción?

Sí. Windows Admin Center generalmente está disponible y listo para implementaciones amplias de uso y de producción. Las capacidades de la plataforma actual y las principales herramientas cumplen los criterios de versión estándar de Microsoft y nuestra barra de calidad de facilidad de uso, confiabilidad, rendimiento, accesibilidad, seguridad y adopción.

[!INCLUDE [support-policy](../includes/support-policy.md)]

## ¿Cuánto cuesta usar Windows Admin Center?

Windows Admin Center no tiene ningún coste adicional más allá de Windows. Puedes usar Windows Admin Center (disponible como una descarga independiente) con licencias válidas de Windows Server o Windows 10 sin coste adicional; se concede bajo licencia en un EULA complementario de Windows.

## ¿Qué versiones de Windows Server puedo administrar con Windows Admin Center?

Windows Admin Center está optimizado para que Windows Server 2019 a habilitará temas claves en la versión de Windows Server 2019: híbridos en la nube escenarios y administración de la infraestructura hiperconvergida en particular. Aunque Windows Admin Center funcionará mejor con Windows Server 2019, permite administrar una variedad de versiones que los clientes ya usan: Windows Server 2012 y versiones posterior son totalmente compatibles. También hay una funcionalidad limitada para administrar Windows Server 2008 R2.

## ¿Es Windows Admin Center un reemplazo completo para todos las herramientas RSAT y predeterminadas tradicionales?

No. Aunque Windows Admin Center puede administrar muchos escenarios comunes, no reemplaza completamente todas las herramientas tradicionales de Microsoft Management Console (MMC). Para una visión detallada de las herramientas que se incluyen con Windows Admin Center, lee más acerca de cómo [Administrar servidores](..\use\manage-servers.md) en nuestra documentación. Windows Admin Center tiene las siguientes capacidades claves de su solución de Administrador de servidores:

* Visualización y utilización de recursos
* Administración de certificados
* Administración de dispositivos
* Visor de eventos
* Explorador de archivos
* Administración de Firewall
* Administración de aplicaciones instaladas
* Configuración de grupos y usuarios locales
* Configuración de red
* Visualización y finalización de procesos y creación de volcados de proceso
* Edición del registro
* Administrar tareas programadas
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

## ¿Puede utilizarse Windows Admin Center para administrar el servidor gratuito de Microsoft Hyper-V?

Sí. Windows Admin Center puede usarse para administrar Microsoft Hyper-V Server 2016 y Microsoft Hyper-V Server 2012 R2.

## ¿Puedo implementar Windows Admin Center en un equipo Windows 10?

Sí, Windows Admin Center se puede instalar en Windows 10 (versión 1709 o posterior), ejecutando en modo de escritorio.  Windows Admin Center también se puede instalar en un servidor con Windows Server 2016 o superior en el modo de puerta de enlace y, a continuación, acceder a través de un explorador web desde un equipo Windows 10. [Obtén más información acerca de las opciones de instalación](..\plan\installation-options.md).

## He oído que Windows Admin Center usa PowerShell avanzada, ¿¿se puede ver los scripts reales que usa?

¡Sí! la [característica de Showscript](..\use\get-started.md#view-powershell-scripts-used-in-windows-admin-center) se agregó en versión preliminar 1806 de Windows Admin Center y ahora se incluye en el canal de disponibilidad general.

## ¿Existen planes para que Windows Admin Center administre Windows Server 2008 R2 o una versión anterior?

Windows Admin Center ahora admite una funcionalidad **limitada** para administrar Windows Server 2008 R2. Windows Admin Center se basa en funciones de PowerShell y las tecnologías de plataforma que no existen en Windows Server 2008 R2 y versiones anteriores, lo que hace que el soporte completo no sea factible. Windows Server 2008/2008 R2 se aproxima al fin de soporte en enero de 2020 por lo que Microsoft recomienda a los clientes [trasladarse a Azure o actualizar a la versión más reciente de Windows Server](https://www.microsoft.com/en-us/cloud-platform/windows-server-2008).

## ¿Existen planes para que Windows Admin Center administre conexiones de Linux?

Estamos investigando debido a la demanda de clientes, pero actualmente no hay ningún plan bloqueado de entrega y soporte técnico puede que conste solo de una conexión de consola a través de SSH.

## ¿Qué exploradores web son compatibles con Windows Admin Center?

Las versiones más recientes de Microsoft Edge (Windows 10, versión 1709 o posterior) y los exploradores Google Chrome están probadas y se admiten en Windows 10. [Problemas conocidos de la vista Explorador específicos](..\support\known-issues.md#browser-specific-issues). Otros exploradores web modernos u otras plataformas actualmente no forman parte de nuestra matriz de pruebas y, por tanto, no son *oficialmente* compatible.

## ¿Cómo controla Windows Admin Center la seguridad?

El tráfico desde el explorador a la puerta de enlace de Windows Admin Center usa HTTPS. El tráfico desde la puerta de enlace a los servidores administrados es PowerShell y WMI estándar a través de WinRM. Admitimos LAPS (Solución de contraseñas de administrador local), delegación limitada basada en recursos, control de acceso de puerta de enlace mediante AD o Azure AD, y control de acceso basado en roles para administrar servidores de destino.

## ¿Windows Admin Center usa CredSSP?

Sí, en algunos casos, Windows Admin Center requiere CredSSP. Esto es necesario para pasar las credenciales para la autenticación a máquinas más allá del servidor específico de destino para la administración. Por ejemplo, si está administrando máquinas virtuales en **el servidor B**, pero quieres guardar los archivos de vhdx de esas máquinas virtuales en un recurso compartido de archivo hospedado por **servidor C**, Windows Admin Center debe usar CredSSP para autenticar con **el servidor C** para tener acceso a la recurso compartido de archivos.

Windows Admin Center administra la configuración de CredSSP automáticamente después de solicitar el consentimiento del usuario. Antes de configurar CredSSP, Windows Admin Center comprobará para asegurarte de que el sistema tiene las CredSSP recientes [actualizaciones](https://support.microsoft.com/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018). Mientras CredSSP está habilitada, habrá una notificación en la información general del servidor y una opción para deshabilitarla:

![CredSSP en información general del servidor](../media/CredSSP-overview.png)

CredSSP actualmente se usa en las siguientes áreas:

- Usar desagregadas almacenamiento SMB en la herramienta de máquinas virtuales (el ejemplo anterior).
- Uso de las actualizaciones de la herramienta en la conmutación por error o clústeres Hyper soluciones de administración, que realiza [La actualización compatible con clústeres](https://docs.microsoft.com/windows-server/failover-clustering/cluster-aware-updating) 

## ¿Hay alguna dependencia de la nube?

Windows Admin Center no requiere acceso a Internet y no requiere Microsoft Azure. Windows Admin Center administra Windows Server e instancias de Windows en cualquier lugar: en sistemas físicos o en máquinas virtuales en cualquier hipervisor o ejecutándose en cualquier nube. Aunque con el tiempo se agregará la integración con distintos servicios de Azure, estas serán características de valor añadido opcionales y no es un requisito para usar Windows Admin Center.

## ¿Existen otras dependencias o requisitos previos?

Windows Admin Center se puede instalar en Windows 10 Fall Anniversary Update (1709) o versión más reciente, o Windows Server 2016 o versión más reciente. Para administrar Windows Server 2008 R2, 2012 o 2012 R2, se requiere la instalación de Windows Management Framework 5.1 en esos servidores. No existen otras dependencias. No se requiere IIS, no se requieren agentes, no se requiere SQL Server.

## ¿Qué sucede con la extensibilidad y la compatibilidad con proveedores de terceros?

Windows Admin Center tiene un SDK disponible para que cualquier persona puede escribir su propia extensión. Como plataforma, aumentar nuestro ecosistema y posibilitar la extensibilidad de partners ha sido una prioridad clave desde el principio. [Más información acerca del SDK de Windows Admin Center](..\extend\extensibility-overview.md).

## ¿Puedo administrar la infraestructura hiperconvergida con Windows Admin Center?

Sí. Windows Admin Center es compatible con la administración de clústeres hiperconvergidos que ejecutan Windows Server 2016 o Windows Server 2019. La solución del Administrador de clústeres en Windows Admin Center se estableció anteriormente en la vista previa, pero ahora está **disponible para el público**, con algunas nuevas funciones en vista previa. Para obtener más información, [lee más acerca de la administración de la infraestructura hiperconvergida](..\use\manage-hyper-converged.md).

## ¿Necesita Windows Admin Center System Center?

No. Windows Admin Center es complementario para System Center, pero System Center no es necesario. [Más información acerca del centro de administración de Windows y System Center](related-management.md#system-center).

## ¿Puede reemplazar Windows Admin Center a System Center Virtual Machine Manager (SCVMM)?

Windows Admin Center y SCVMM son complementarias; Windows Admin Center está pensado para reemplazar los complementos tradicionales de Microsoft Management Console (MMC) y la experiencia de administración de servidores.  Windows Admin Center no está pensado para sustituir los aspectos de supervisión de SCVMM. [Más información acerca del centro de administración de Windows y System Center](related-management.md#system-center).

## ¿Qué es la versión preliminar de Windows Admin Center, qué versión es la adecuada para mí?

Hay dos versiones de Windows Admin Center disponibles para su descarga:

### Windows Admin Center

* Esta versión es para administradores de TI que no pueden actualizar con frecuencia o que desean más tiempo de validación para las versiones que se utilizan en producción. Nuestra versión (GA) disponible para el público actual es Windows Admin Center 1904.
* [!INCLUDE [support-policy](../includes/support-policy.md)]
* Para obtener la versión más reciente, [Descárgala aquí](https://aka.ms/WACDownload).

### Versión preliminar de Windows Admin Center

>[!NOTE]
>La versión de disponibilidad general actual (Windows Admin Center 1904) contiene toda la funcionalidad de vista previa anterior.
>Insider Preview devolverá en los próximos meses.

* Esta versión es para administradores de TI que desean las características mejores y más recientes en una cadencia regular. Nuestra intención es proporcionar versiones de actualización posteriores cada mes aproximadamente. La plataforma principal sigue estando lista para la producción y la licencia proporciona derechos de uso de producción. Sin embargo, ten en cuenta que podrás ver la introducción de nuevas herramientas y funciones que están marcadas claramente como VERSIÓN PRELIMINAR y son adecuadas para evaluar y probar.
* Para obtener la última versión preliminar de Insider, los Insider registrados pueden descargar la versión preliminar de Windows Admin Center directamente desde la [Página de descarga de Windows Server Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver), en la lista desplegable de descargas adicionales. Si aún no te has registrado como usuario de Insider, consulta la [Introducción a Windows Server](https://insider.windows.com/en-us/for-business-getting-started-server/) en el portal de Windows Insider para empresas.

## ¿Por qué se ha seleccionado "Windows Admin Center" como el nombre final para el "Proyecto Honolulu"?

Windows Admin Center es el nombre oficial del producto del "Proyecto Honolulu" y refuerza nuestra visión de una experiencia integrada para los administradores de TI a través de una gran variedad de escenarios administrativos y de gestión. También destaca nuestro enfoque centrado en el cliente en las necesidades de los usuarios de administración de TI como fundamentales para la forma en que invertimos y lo que ofrecemos.

## ¿Dónde puedo obtener más información Windows Admin Center u obtener información más detallada sobre los temas anteriores?

Nuestra [página de inicio](https://aka.ms/WindowsAdminCenter) es el mejor punto de partida e incluye vínculos al contenido recientemente clasificado de nuestra documentación, la ubicación de descarga, cómo proporcionar comentarios, información de referencia y otros recursos.

## ¿Qué es el historial de versiones de Windows Admin Center?

[Ver el historial de versión aquí.](..\overview.md#release-history)

## Tengo un problema con Windows Admin Center, ¿dónde puedo obtener ayuda?

Consulta nuestra [Guía de solución de problemas](..\use\troubleshooting.md) y nuestra lista de [problemas conocidos](..\use\known-issues.md).

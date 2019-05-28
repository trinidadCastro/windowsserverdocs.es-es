---
title: What ' s New in Windows Server, versión 1903
description: En este tema se describe algunas de las nuevas características de Windows Server, versión 1903, que es una versión de canal semianual.
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.date: 05/21/2019
ms.openlocfilehash: 2aa84df1ca162cb6e2dfa580b4b0744ff08cc95a
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65983445"
---
# <a name="whats-new-in-windows-server-version-1903"></a>Novedades de Windows Server, versión 1903

>Se aplica a: Windows Server (Canal semianual)

En este tema se describe algunas de las nuevas características de Windows Server, versión 1903, que es una versión de canal semianual. Estas características incluyen mejoras para ejecutar y administrar contenedores, las herramientas para trabajar en las instalaciones Server Core y la capacidad de migrar el almacenamiento de los dispositivos de Linux.

Para buscar en su lugar las novedades en la versión más reciente de canal de mantenimiento a largo plazo (LTSC) de Windows Server, consulte vea [What ' s New in Windows Server 2019](../get-started-19/whats-new-19.md). Consulte también [cuáles son las novedades en Windows 10, el contenido para profesionales de TI de versión 1903](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1903).

Los requisitos del sistema para esta versión son los mismos que para Windows Server 2019: vea [requisitos del sistema](../get-started-19/sys-reqs-19.md) para obtener más información. Para ver lo que se va a quitar recientemente, consulte [características eliminadas o planeada para el reemplazo a partir de Windows Server, versión 1903](../get-started-19/removed-features-1903.md)

> [!NOTE]
> Contenedores de Windows deben usar la misma versión de Windows como el servidor host, o un *anteriores* versión. Por ejemplo, un servidor de host que ejecuta la versión comercial de Windows Server, versión 1903 (compilación 18342) puede ejecutar contenedores de Windows Server con la versión de la misma o una versión anterior y número de compilación (incluso si el contenedor usa una versión preliminar de Insider de Windows). Para obtener más información, consulte [compatibilidad de las versiones Windows contenedor](https://docs.microsoft.com/virtualization/windowscontainers/deploy-containers/version-compatibility).

## <a name="enhanced-support-for-non-microsoft-container-services"></a>Compatibilidad mejorada para servicios de contenedor que no sean de Microsoft

Se han mejorado las capacidades de plataforma para admitir servicios de contenedor de Azure y servicios de contenedor que no sean de Microsoft.

- Integramos CRI containerd con Host Compute Service (HCS) para admitir los pods de contenedores de Windows y Linux en Windows (LCOW) en Azure.
- Hemos trabajado con la Comunidad de Kubernetes para habilitar la compatibilidad con contenedores de Windows. Con la versión de Kubernetes v1.14, soporte del nodo de Windows Server se graduó oficialmente en beta a estable. Para obtener más información, consulte [contenedores de Windows ahora se admiten en Kubernetes](https://cloudblogs.microsoft.com/opensource/2019/03/25/windows-server-containers-now-supported-kubernetes/).
- Ahora está disponible con carácter general como parte de la suscripción Tigera Essentials y ofertas Tigera Calico para Windows mixto de directiva de red y redes de no superposición interoperable entre entornos Windows o Linux.
- Se entregan las mejoras de escalabilidad para mejorar la superposición de red para los contenedores de Windows, incluida la integración con Kubernetes a través de la versión más reciente de Franela y Kubernetes v1.14. Para obtener más información, consulte [Introducción al soporte técnico de Windows en Kubernetes](https://kubernetes.io/docs/setup/windows/).

## <a name="directx-hardware-acceleration-in-containers"></a>Aceleración de hardware de DirectX en contenedores

Soporte técnico para la aceleración de hardware de DirectX APIs en escenarios de compatibilidad con contenedores de Windows tp, como la inferencia de Machine Learning (ML) mediante hardware (GPU) de la unidad de procesamiento gráfico local permitimos el acceso. Para obtener más información, consulte [aceleración GPU Traer a contenedores de Windows](https://techcommunity.microsoft.com/t5/Containers/Bringing-GPU-acceleration-to-Windows-containers/ba-p/393939).

## <a name="updated-container-identity-and-group-managed-service-account-documentation"></a>Identidad de contenedor actualizado y el grupo administran de documentación de la cuenta de servicio

Hemos agregado más ejemplos e información de compatibilidad para la [cuentas de servicio administradas de grupo](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts) documentación y realiza el [módulo de PowerShell de la especificación de credenciales](https://www.powershellgallery.com/packages/CredentialSpec) disponibles en la Galería de PowerShell. Para obtener más información, consulte el [Novedades de la identidad de contenedor](https://techcommunity.microsoft.com/t5/Containers/What-s-new-for-container-identity/ba-p/389151) entrada de blog.

## <a name="add-task-scheduler-and-hyper-v-manager-to-server-core-installations"></a>Agregar administrador de Hyper-V y de programador de tareas para las instalaciones Server Core

Como ya sabe, se recomienda usar la opción de instalación Server Core cuando se usa Windows Server, el canal semianual en producción. Sin embargo, Server Core de forma predeterminada se omite un número de herramientas de administración útiles. Puede agregar muchas de las más frecuente herramientas instalando la característica de compatibilidad de aplicaciones, pero todavía ha habido algunas herramientas que faltan.

Por lo tanto, según los comentarios recibidos, hemos agregado dos herramientas más a la característica de compatibilidad de aplicaciones en esta versión: El programador de tareas (taskschd.msc) y Administrador de Hyper-V (virtmgmt.msc).

Para obtener más información, consulte [característica de compatibilidad de aplicaciones de Server Core](../get-started-19/install-fod-19.md).

## <a name="storage-migration-service-now-migrates-local-accounts-clusters-and-linux-servers"></a>Servicio de migración de almacenamiento ahora migra las cuentas locales, clústeres y servidores Linux

Servicio de migración de almacenamiento facilita la migración de servidores a una versión más reciente de Windows Server. Proporciona una herramienta gráfica que hará un inventario de datos en los servidores y, a continuación, transfiere los datos y la configuración a los servidores más recientes, todo ello sin tener que cambiar nada de usuarios o aplicaciones.

Al usar esta versión de Windows Server para organizar las migraciones, hemos agregado las siguientes capacidades:

- Migrar usuarios y grupos locales en el nuevo servidor
- Migrar el almacenamiento de clústeres de conmutación por error
- Migrar el almacenamiento desde un servidor de Linux que usa Samba
- Sincronizar más fácilmente recursos compartidos migrados en Azure mediante el uso de Azure File Sync
- Migrar a nuevas redes, como Azure

Para obtener más información acerca del servicio de migración de almacenamiento, consulte [información general del servicio de migración de almacenamiento](../storage/storage-migration-service/overview.md).

## <a name="system-insights-disk-anomaly-detection"></a>Detección de anomalías de disco de sistema Insights

[Información del sistema](../manage/system-insights/overview.md) es una característica de análisis predictivo que analiza los datos del sistema de Windows Server localmente y proporciona información detallada sobre el funcionamiento del servidor. Incluye una serie de funcionalidades integradas, pero hemos agregado la capacidad para instalar funciones adicionales a través de Windows Admin Center, a partir de la detección de anomalías de disco.

Detección de anomalías de disco es una nueva funcionalidad que resalta cuando se comportan discos *diferente* de lo habitual. Aunque diferentes no es necesariamente algo malo, ver estos instantes anómalas puede resultar útil al solucionar problemas en sus sistemas.

Esta funcionalidad también está disponible para los servidores que ejecutan Windows Server 2019.

## <a name="windows-admin-center-enhancements"></a>Mejoras de Windows Admin Center

Una nueva versión de Windows Admin Center es horizontal, agregar nueva funcionalidad a Windows Server. Para obtener información sobre las características más recientes, consulte [Windows Admin Center](../manage/windows-admin-center/understand/windows-admin-center.md).

## <a name="security-baseline-for-windows-10-and-windows-server"></a>Línea de base de seguridad para Windows 10 y Windows Server

La versión de borrador de la [opciones de línea de base de configuración de seguridad](https://blogs.technet.microsoft.com/secguide/2019/04/24/security-baseline-draft-for-windows-10-v1903-and-windows-server-v1903/) para Windows 10 versión 1903 y para Windows Server versión 1903 está disponible.

## <a name="setupdiag"></a>SetupDiag
[SetupDiag](https://docs.microsoft.com/windows/deployment/upgrade/setupdiag) versión 1.4.1 está disponible.

SetupDiag es una herramienta de línea de comandos que puede ayudar a diagnosticar por qué no se pudo una actualización de Windows. SetupDiag funciona mediante la búsqueda de archivos de registro de la instalación de Windows. Al buscar en los archivos de registro, SetupDiag usa un conjunto de reglas que coincidan con los problemas conocidos. En la versión actual de SetupDiag hay 53 reglas contenidas en el archivo rules.xml, que se extrae cuando se ejecuta SetupDiag. El archivo rules.xml se actualizará a medida que se vuelvan disponibles nuevas versiones de SetupDiag.

## <a name="update-rollback-improvements"></a>Mejoras en la reversión de actualización

Los servidores ahora pueden recuperarse automáticamente de errores de inicio mediante la eliminación de las actualizaciones si se presentó el error de inicio tras la instalación de actualizaciones de calidad o controlador recientes. Cuando un dispositivo no puede iniciarse correctamente después de la instalación reciente de calidad de las actualizaciones de controladores, Windows ahora desinstalará automáticamente las actualizaciones para obtener el dispositivo de copia de seguridad y la ejecuta con normalidad.

Esta funcionalidad requiere que el servidor a usar la opción de instalación Server Core con un [entorno de recuperación de Windows](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference) partición.

## <a name="microsoft-defender-advanced-threat-protection-atp-improvements"></a>Mejoras de protección de amenazas avanzada de Microsoft Defender (ATP)

Windows Server incluye protección Microsoft Defender avanzada de subprocesos (para obtener más información, consulte [Antivirus de Windows Defender en Windows Server](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016)). Esta versión incluye las siguientes mejoras:

- [Reducción de superficie expuesta a ataques](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview-attack-surface-reduction) : los administradores de TI pueden configurar los dispositivos con protección de web avanzadas que les permite definir permitir y denegar listas de direcciones IP y direcciones URL específicas.
- [Protección de próxima generación](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10) : los controles se han ampliado a la protección de ransomware, uso indebido de credenciales y los ataques que se transmiten a través de almacenamiento extraíble.
    - Funcionalidades de integridad cumplimiento – Enable en tiempo de ejecución remoto atestación.
    - Altera y capacidades: utiliza la seguridad basada en virtualización para aislar las capacidades de seguridad de ATP críticas a del sistema operativo y los atacantes.
- Tecnologías de protección de última generación de Microsoft Defender ATP:
    - **Avanzada de aprendizaje automático**: Se ha mejorado con modelos de inteligencia artificial que habilitarla para protegerse frente a los atacantes de vértice con malware, herramientas y técnicas de explotación de vulnerabilidades innovadoras y aprendizaje automático avanzado.
    - **Protección de brote de emergencia**: Proporciona protección de brote de emergencia que actualizará automáticamente los dispositivos con nueva inteligencia cuando ha detectado un ataque de nuevo.
    - **Certificación ISO 27001 cumplimiento**: Garantiza que el servicio en la nube se analiza las amenazas, vulnerabilidades y efectos, y que son controles de seguridad y administración de riesgos en su lugar.
    - **Compatibilidad con la ubicación geográfica**: Admite la ubicación geográfica y soberanía de datos de ejemplo, así como las directivas de retención configurable.
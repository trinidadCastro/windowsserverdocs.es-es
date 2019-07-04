---
title: Novedades de Windows Server, versión 1903
description: En este tema se describen algunas de las nuevas funciones de Windows Server, versión 1903.
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.date: 05/21/2019
ms.openlocfilehash: 0ec6a7ec624818b92fb306089f3dea3c786c0827
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280314"
---
# <a name="whats-new-in-windows-server-version-1903"></a>Novedades de Windows Server, versión 1903

>Se aplica a: Windows Server (Canal semianual)

En este tema se describen algunas de las nuevas funciones de Windows Server, versión 1903. Estas características incluyen mejoras para ejecutar y administrar contenedores, herramientas para trabajar en instalaciones básicas y la capacidad de migrar el almacenamiento de los dispositivos Linux.

Para ver las novedades de la versión más reciente de Canal de mantenimiento a largo plazo (LTSC) de Windows Server, consulta [Novedades de Windows Server 2019](../get-started-19/whats-new-19.md). Consulta también [Novedades de Windows 10, contenido profesional de TI de la versión 1903](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1903).

Los requisitos del sistema para esta versión son los mismos que para Windows Server 2019. Consulta [Requisitos del sistema](../get-started-19/sys-reqs-19.md) para más información. Para ver qué se ha eliminado recientemente, consulta [Características eliminadas o planificadas para su reemplazo a partir de Windows Server (versión 1903)](../get-started-19/removed-features-1903.md).

> [!NOTE]
> Los contenedores Windows deben usar la misma versión de Windows que el servidor host, o una versión *anterior*. Por ejemplo, un servidor host que ejecuta la versión comercial de Windows Server, versión 1903 (compilación 18342) puede ejecutar contenedores Windows Server con un número de versión y compilación igual o anterior (aunque el contenedor use una versión de Insider Preview de Windows). Para más información, consulta [Compatibilidad con versiones de contenedores Windows](https://docs.microsoft.com/virtualization/windowscontainers/deploy-containers/version-compatibility).

## <a name="enhanced-support-for-non-microsoft-container-services"></a>Compatibilidad mejorada para servicios de contenedor de terceros

Se han mejorado las funcionalidades de la plataforma para admitir servicios de contenedor de Azure y servicios de contenedor de terceros.

- Hemos integrado CRI containerd con Host Compute Service (HCS) para admitir en Azure pods de contenedores Windows y contenedores Linux en Windows (LCOW).
- Hemos trabajado con la comunidad de Kubernetes para habilitar la compatibilidad con contenedores Windows. Con la versión de Kubernetes v1.14, la compatibilidad con los nodos de Windows Server ha pasado oficialmente de beta a estable. Para más información, consulta [Windows containers now supported in Kubernetes](https://cloudblogs.microsoft.com/opensource/2019/03/25/windows-server-containers-now-supported-kubernetes/) (Kubernetes ya admite contenedores Windows).
- Tigera Calico para Windows ya está disponible con carácter general como parte de la suscripción Tigera Essentials y ofrece directiva de red y redes sin superposición que funcionan en entornos Linux/Windows mixtos.
- Hemos incorporado mejoras en la escalabilidad para mejorar la compatibilidad con las redes de superposición para los contenedores Windows, incluida la integración con Kubernetes mediante la versión más reciente de Flannel y Kubernetes v1.14. Para más información, consulta [Intro to Windows support in Kubernetes](https://kubernetes.io/docs/setup/windows/) (Introducción a la compatibilidad con Windows en Kubernetes).

## <a name="directx-hardware-acceleration-in-containers"></a>Aceleración de hardware de DirectX en contenedores

Estamos habilitando la compatibilidad con la aceleración de hardware de las API de DirectX en contenedores Windows para admitir escenarios como la inferencia de aprendizaje automático mediante hardware de unidad de procesamiento gráfico (GPU) local. Para más información, consulta [Bringing GPU acceleration to Windows containers](https://techcommunity.microsoft.com/t5/Containers/Bringing-GPU-acceleration-to-Windows-containers/ba-p/393939) (Incorporar aceleración GPU a los contenedores Windows).

## <a name="updated-container-identity-and-group-managed-service-account-documentation"></a>Actualización de la documentación sobre identidad de contenedor y cuentas de servicio administradas de grupo

Hemos agregado más ejemplos e información sobre compatibilidad a la documentación de [cuentas de servicio administradas de grupo](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts) y el [módulo Credential Spec de PowerShell](https://www.powershellgallery.com/packages/CredentialSpec) ahora está disponible en la Galería de PowerShell. Para más información, consulta la entrada de blog [What's new for container identity](https://techcommunity.microsoft.com/t5/Containers/What-s-new-for-container-identity/ba-p/389151) (Novedades de la identidad de contenedor).

## <a name="add-task-scheduler-and-hyper-v-manager-to-server-core-installations"></a>Agregar el administrador de Hyper-V y el Programador de tareas a las instalaciones básicas

Como ya sabes, se recomienda usar la opción de instalación básica cuando se usa el canal semianual en producción de Windows Server. Sin embargo, de manera predeterminada, la instalación básica omite algunas herramientas de administración útiles. Para agregar muchas de las herramientas que se usan con más frecuencia, puedes instalar la característica App Compatibility, pero aún así faltan algunas herramientas.

Por lo tanto, según los comentarios recibidos, hemos agregado dos herramientas más a la característica App Compatibility en esta versión: Programador de tareas (taskschd.msc) y Administrador de Hyper-V (virtmgmt.msc).

Para más información, consulta [Característica App Compatibility de Server Core](../get-started-19/install-fod-19.md).

## <a name="storage-migration-service-now-migrates-local-accounts-clusters-and-linux-servers"></a>El servicio de migración de almacenamiento ahora migra las cuentas locales, los clústeres y los servidores Linux

El servicio de migración de almacenamiento facilita la migración de servidores a una versión más reciente de Windows Server. Proporciona una herramienta gráfica que hace un inventario de los datos en los servidores y, a continuación, transfiere los datos y la configuración a los servidores más recientes, todo ello sin que los usuarios o las aplicaciones tengan que cambiar nada.

Hemos agregado las siguientes funcionalidades para usar esta versión de Windows Server para organizar las migraciones:

- Migrar usuarios y grupos locales al nuevo servidor.
- Migrar el almacenamiento de los clústeres de conmutación por error.
- Migrar el almacenamiento desde un servidor Linux que usa Samba.
- Sincronizar más fácilmente los recursos compartidos migrados en Azure mediante Azure File Sync.
- Migrar a nuevas redes, como Azure.

Para más información acerca del servicio de migración de almacenamiento, consulta [Información general sobre el servicio de migración de almacenamiento](../storage/storage-migration-service/overview.md).

## <a name="system-insights-disk-anomaly-detection"></a>Detección de anomalías de disco con Información del sistema

[Información del sistema](../manage/system-insights/overview.md) es una característica de análisis predictivo que analiza los datos del sistema Windows Server localmente y proporciona información detallada sobre el funcionamiento del servidor. Incluye varias funcionalidades integradas, pero hemos agregado la capacidad para instalar funcionalidades adicionales mediante Windows Admin Center, empezando por la detección de anomalías de disco.

La detección de anomalías de disco es una nueva funcionalidad que avisa cuando los discos se comportan *de una forma diferente* a la habitual. Aunque diferentes no significa necesariamente algo malo, ver estas anomalías puede resultar útil para solucionar problemas en los sistemas.

Esta funcionalidad también está disponible para los servidores que ejecutan Windows Server 2019.

## <a name="windows-admin-center-enhancements"></a>Mejoras de Windows Admin Center

Hay una nueva versión de Windows Admin Center disponible, que aporta nueva funcionalidad a Windows Server. Para obtener información sobre las características más recientes, consulta [Windows Admin Center](../manage/windows-admin-center/understand/windows-admin-center.md).

## <a name="security-baseline-for-windows-10-and-windows-server"></a>Base de referencia de seguridad para Windows 10 y Windows Server

Ya está disponible la versión de borrador de la [configuración de seguridad de referencia](https://blogs.technet.microsoft.com/secguide/2019/04/24/security-baseline-draft-for-windows-10-v1903-and-windows-server-v1903/) para Windows 10 versión 1903 y Windows Server versión 1903.

## <a name="setupdiag"></a>SetupDiag
Está disponible [SetupDiag](https://docs.microsoft.com/windows/deployment/upgrade/setupdiag) versión 1.4.1.

SetupDiag es una herramienta de línea de comandos que puede ayudar a diagnosticar por qué se produjo un error en una actualización de Windows. SetupDiag funciona mediante la búsqueda de archivos de registro de la instalación de Windows. Al buscar en los archivos de registro, SetupDiag usa un conjunto de reglas que coincidan con los problemas conocidos. En la versión actual de SetupDiag, hay 53 reglas incluidas en el archivo rules.xml, que se extrae al ejecutar SetupDiag. El archivo rules.xml se actualizará a medida que se vuelvan disponibles nuevas versiones de SetupDiag.

## <a name="update-rollback-improvements"></a>Mejoras en la reversión de la actualización

Ahora, los servidores pueden recuperarse automáticamente de errores de inicio mediante la eliminación de las actualizaciones si el error se produjo después de instalar actualizaciones de calidad o de controladores. Cuando un dispositivo no puede iniciarse correctamente después de una instalación reciente de actualizaciones de calidad o de controladores, Windows desinstalará automáticamente las actualizaciones para que el dispositivo vuelva a funcionar con normalidad.

Esta funcionalidad requiere que el servidor use la opción de instalación básica con una partición [Entorno de recuperación de Windows](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

## <a name="microsoft-defender-advanced-threat-protection-atp-improvements"></a>Protección contra amenazas avanzada de Microsoft Defender (ATP)

Windows Server incluye Protección contra amenazas avanzada de Microsoft Defender (para más información, consulta [Antivirus de Windows Defender en Windows Server](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016)). Esta versión incluye las siguientes mejoras:

- [Reducción de la superficie expuesta a ataques](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview-attack-surface-reduction): los administradores de TI pueden configurar los dispositivos con protección web avanzada que les permite definir listas de direcciones IP y URL específicas permitidas y no permitidas.
- [Protección de próxima generación](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10): se han ampliado los controles para la protección contra ransomware, uso indebido de credenciales y ataques que se transmiten a través de almacenamiento extraíble.
    - Funcionalidades de integridad cumplimiento: habilita la atestación remota en tiempo de ejecución.
    - Funcionalidades de comprobación de alteraciones: utiliza la seguridad basada en virtualización para que el sistema operativo y los atacantes no tengan acceso a las funcionalidades de seguridad de ATP críticas.
- Tecnologías de protección de última generación de Microsoft Defender ATP:
    - **Aprendizaje automático avanzado**: se ha mejorado con modelos de inteligencia artificial y aprendizaje automático avanzado que permite protegerse frente a los atacantes que usan innovadoras técnicas, herramientas y malware de explotación de vulnerabilidades.
    - **Protección contra ataques de emergencia**: proporciona protección contra ataques de emergencia y actualizará automáticamente los dispositivos con nueva inteligencia cuando se detecten nuevos ataques.
    - **Cumplimiento de la certificación ISO 27001**: garantiza que el servicio en la nube ha analizado las amenazas, las vulnerabilidades y los efectos, y que hay en marcha controles de seguridad y administración de los riesgos.
    - **Compatibilidad con geolocalización**: admite la geolocalización y la soberanía de los datos de ejemplo, así como las directivas de retención configurables.
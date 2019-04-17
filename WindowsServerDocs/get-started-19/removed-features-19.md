---
title: Características eliminadas o planificadas para su eliminación en Windows Server 2019
description: Obtén información sobre las características y funcionalidades eliminadas o planificadas para su eliminación a partir de Windows Server 2019.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: jasgro
ms.date: 03/29/2019
ms.localizationpriority: medium
ms.openlocfilehash: 470857616a9b36d238de031b4ccf80a68eff1e61
ms.sourcegitcommit: 971f6538e8d89af84ef50fc8aab2188bdf6f47cb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2019
ms.locfileid: "9279136"
---
# <a name="features-removed-or-planned-for-replacement-starting-windows-server-2019"></a>Características eliminadas o planificadas para su reemplazo a partir de Windows Server 2019

>Se aplica a: Windows Server2019

Cada versión de Windows Server agrega nuevas características y funciones; ocasionalmente podemos eliminar características y funciones, normalmente debido a que hemos agregado una mejor opción. Estos son los detalles sobre las características y funcionalidades que hemos eliminado en Windows Server 2019.   

> [!TIP]
> - Puedes obtener acceso rápido a las versiones de Windows Server uniéndote al [Programa Windows Insider](https://insider.windows.com): esta es una excelente manera de probar los cambios de características.
> - ¿Tienes preguntas acerca de otras versiones? Revisa la información de [Windows Server, versión 1803](../get-started/windows-server-1803-removed-features.md), [Windows Server, versión 1709](../get-started/removed-features-1709.md)y [Windows Server 2016](../get-started/deprecated-features.md).

**La lista está sujeta a cambios y podría no incluir todas las características o funciones afectadas.** 

## <a name="features-we-removed-in-this-release"></a>Características que hemos eliminado en esta versión

Nos estamos quitando las siguientes características y funcionalidades de la imagen de producto instalada en Windows Server 2019. Las aplicaciones o código que dependen de estas características no funcionarán en esta versión a menos que uses un método alternativo.   

|Característica    |En su lugar, puedes usar...|
|-----------|--------------------
|Análisis de negocio, que también se denomina Administración de digitalización distribuida (DSM)|Nos estamos quitando esta digitalización seguro y la capacidad de administración de escáner: no hay ningún dispositivo que admiten esta característica.|
|Imprimir componentes - componente ahora opcional para las instalaciones de Server Core|En versiones anteriores de Windows Server, los componentes de impresión eran *deshabilitado* de manera predeterminada en la opción de instalación Server Core. Hemos cambiado en Windows Server 2016, lo que les permite de forma predeterminada. En Windows Server 2019, una vez más se deshabilitan los componentes de impresión de manera predeterminada de Server Core. Si es necesario habilitar los componentes de impresión, puede hacerlo mediante la ejecución del cmdlet **WindowsFeature de instalación de servidor de impresión** .|
|[Agente de conexión a Escritorio remoto y Host de virtualización de escritorio remoto](../remote/remote-desktop-services/desktop-hosting-service.md) en una instalación de Server Core|La mayoría de las implementaciones de servicios de escritorio remoto tienen estos roles colocalizados con el host de sesión de Escritorio remoto (RDSH), que requiere el Servidor con Experiencia de escritorio; para que sea coherente con RDSH vamos a cambiar estos roles para también requerir Servidor con Experiencia de escritorio. Estos roles RDS ya no están disponibles para su uso en una [instalación Server Core](../administration/server-core/what-is-server-core.md). Si es necesario [implementar estos roles como parte de la infraestructura de escritorio remoto](../remote/remote-desktop-services/rds-deploy-infrastructure.md), puedes [instalarlos en Windows Server con experiencia de escritorio](../get-started/getting-started-with-server-with-desktop-experience.md). <br/><br/>Estas funciones también se incluyen en la opción de instalación de Experiencia de escritorio de Windows Server 2019. |



## <a name="features-were-no-longer-developing"></a>Características que ya no se estamos desarrollando

Te estás desarrollando ya no activamente estas características y se pueden quitar de una futura actualización. Algunas características se han reemplazado por otras características o funciones, mientras que otras están ahora disponibles de diferentes orígenes. 

Si tienes algún comentario acerca de la sustitución propuesta de cualquiera de estas características, puedes usar la [aplicación Centro de comentarios](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app). 

|Característica    |En su lugar, puedes usar...|
|-----------|---------------------|
|Unidad de almacenamiento de claves en Hyper-V|Ya no estamos trabajando en la característica de unidad de almacenamiento de claves de Hyper-V. Si estás usando las máquinas virtuales de generación 1, echa un vistazo a [La seguridad de virtualización de máquina virtual de generación 1](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/generation-1-virtual-machine-security-settings-for-hyper-v) para obtener información acerca de las opciones en el futuro. Si vas a crear máquinas virtuales nuevas usan las máquinas virtuales de generación 2 con dispositivos TPM para una solución más segura. |
|Consola de administración de confianza Platform Module (TPM)|La información que estaba disponible en la consola de administración de TPM ahora está disponible en la página de [**seguridad del dispositivo**](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/wdsc-device-security) en el [Centro de seguridad de Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/windows-defender-security-center).|
|Modo de atestación de Active Directory de servicio de protección de host|Ya no estamos desarrollando el modo de atestación de servicio de protección de Host Active Directory, en su lugar, hemos agregado un nuevo modo de atestación, [atestación de clave de host](../security/guarded-fabric-shielded-vm/guarded-fabric-create-host-key.md), que es mucho más sencillo e igualmente compatible como atestación basado en Active Directory.  Este nuevo modo proporciona una funcionalidad equivalente con una experiencia de instalación, administración más sencilla y menos dependencias de infraestructura de la atestación de Active Directory. Atestación de clave de host no tiene ningún requisito de hardware adicional más allá de la atestación de Active Directory es necesario, por lo que todos los sistemas existentes seguirán siendo compatibles con el modo de nuevo. Consulta [implementar protegido hosts](../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md) para obtener más información sobre las opciones de atestación.|
|Servicio de OneSync|El servicio de OneSync sincroniza los datos de las aplicaciones de correo electrónico, calendario y contactos. Hemos agregado un motor de sincronización a la aplicación de Outlook que proporciona la misma sincronización.|
|Compatibilidad con las API de compresión diferencial remota|Compatibilidad con las API de compresión diferencial remota habilitados sincronizar datos con un origen remoto con tecnologías de compresión, lo que minimizan la cantidad de datos enviados a través de la red. Esta compatibilidad actualmente no se usa en ningún producto de Microsoft.|
|Extensión de conmutador de filtro ligero WFP|La extensión del conmutador de filtro ligero WFP permite a los desarrolladores crear [extensiones de filtrado de paquetes de red simple para Hyper-V virtual cambiar](https://docs.microsoft.com/en-us/windows-hardware/drivers/network/using-virtual-switch-filtering). Puedes lograr la misma funcionalidad mediante la creación de una extensión de filtrado completa. Como tal, retiraremos esta extensión en el futuro.|


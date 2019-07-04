---
title: Características eliminadas o que está previsto eliminar en Windows Server 2019
description: Obtén información sobre las características y funcionalidades que se han eliminado o que está previsto eliminar a partir de Windows Server 2019.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: jasongerend
ms.author: jgerend
manager: jasgro
ms.date: 05/21/2019
ms.localizationpriority: medium
ms.openlocfilehash: a27fb6bfa0d96d7d3cdd0b9b5bc4912b12b4462b
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280364"
---
# <a name="features-removed-or-planned-for-replacement-starting-windows-server-2019"></a>Características eliminadas o que está previsto eliminar a partir de Windows Server 2019

>Se aplica a: Windows Server 2019

Cada versión de Windows Server agrega nuevas características y funciones; ocasionalmente podemos eliminar características y funciones, normalmente debido a que hemos agregado una mejor opción. Esta es la información sobre las características y funciones que se han eliminado en Windows Server 2019.

> [!TIP]
> - Para obtener acceso rápido a las versiones de Windows Server, puedes unirte al [Programa Windows Insider](https://insider.windows.com). Esta es una excelente manera de probar los cambios de características.
> - ¿Tienes preguntas acerca de otras versiones? Consulta las [novedades de Windows Server](../get-started/whats-new-in-windows-server.md).

**La lista está sujeta a cambios y podría no incluir todas las características o funciones afectadas.** 

## <a name="features-we-removed-in-this-release"></a>Características que hemos eliminado en esta versión

Hemos eliminado las siguientes características y funcionalidades de la imagen del producto instalado en Windows Server 2019. Las aplicaciones o código que dependen de estas características no funcionarán en esta versión a menos que uses un método alternativo.

|Característica    |En su lugar, puedes usar...|
|-----------|--------------------
|Análisis de negocio, que también se denomina Administración de digitalización distribuida (DSM)|Vamos a eliminar esta funcionalidad de escaneado y administración de escáneres porque no hay ningún dispositivo que admita esta característica.|
|Componentes de impresión: ahora es un componente opcional en las instalaciones básicas.|En las versiones anteriores de Windows Server, los componentes de impresión estaban *deshabilitados* de manera predeterminada en la opción de instalación básica. Eso cambió en Windows Server 2016, donde estaban habilitados de manera predeterminada. En Windows Server 2019, esos componentes de impresión han vuelto a deshabilitarse de manera predeterminada en la instalación básica. Si tiene que habilitar los componentes de impresión, ejecute el cmdlet **Install-WindowsFeature Print-Server**.|
|[Agente de Conexión a Escritorio remoto y Host de virtualización de escritorio remoto](../remote/remote-desktop-services/desktop-hosting-service.md) en una instalación básica|La mayoría de las implementaciones de los Servicios de Escritorio remoto tienen estos roles colocalizados con el host de sesión de Escritorio remoto (RDSH), que requiere Windows Server con experiencia de escritorio; para que sea coherente con RDSH vamos a cambiar estos roles para que también requieran Windows Server con experiencia de escritorio. Estos roles de RDS ya no están disponibles en la [instalación básica](../administration/server-core/what-is-server-core.md). Si necesitas [implementar estos roles como parte de la infraestructura de Escritorio remoto](../remote/remote-desktop-services/rds-deploy-infrastructure.md), puedes [instalarlos en Windows Server con experiencia de escritorio](../get-started/getting-started-with-server-with-desktop-experience.md). <br/><br/>Estas funciones también se incluyen en la opción de instalación de la experiencia de escritorio de Windows Server 2019. |

## <a name="features-were-no-longer-developing"></a>Características que ya no se estamos desarrollando

Ya no desarrollamos activamente estas características y puede que se quiten en una futura actualización. Algunas características se han reemplazado por otras características o funciones, mientras que otras están ahora disponibles de diferentes orígenes. 

Si tienes algún comentario acerca de la sustitución propuesta de cualquiera de estas características, puedes usar la [aplicación Centro de opiniones](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app). 

| Característica   | En su lugar, puedes usar... |
|-----------|---------------------|
| Unidad de almacenamiento de claves de Hyper-V|Ya no trabajamos en la característica de unidad de almacenamiento de claves de Hyper-V. Si usas máquinas virtuales de generación 1, consulta [Seguridad de virtualización de máquinas virtuales de generación 1](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/generation-1-virtual-machine-security-settings-for-hyper-v) para más información acerca de las opciones en el futuro. Si vas a crear nuevas máquinas virtuales, usa máquinas virtuales de generación 2 con dispositivos TPM para una solución más segura. |
| Consola de administración del módulo de plataforma segura (TPM)|La información que estaba disponible en la consola de administración de TPM ahora está disponible en la página [**Seguridad del dispositivo**](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/wdsc-device-security) en el [Centro de seguridad de Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/windows-defender-security-center). |
| Modo de atestación de Active Directory en el servicio Host Guardian|Ya no desarrollamos el modo de atestación de Active Directory en el servicio Host Guardian. En su lugar, hemos agregado un nuevo modo de atestación, [atestación de clave de host](../security/guarded-fabric-shielded-vm/guarded-fabric-create-host-key.md), que es mucho más sencillo e igualmente compatible que la atestación basada en Active Directory.  Este nuevo modo proporciona una funcionalidad equivalente con una experiencia de instalación, una administración más simple y menos dependencias de infraestructura que la atestación de Active Directory. La atestación de claves de host no tiene ningún requisito de hardware diferente de la atestación de Active Directory, por lo que todos los sistemas existentes seguirán siendo compatibles con el nuevo modo. Consulta [Implementación de hosts protegidos](../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md) para obtener más información acerca de las opciones de atestación. |
| Servicio OneSync|El servicio OneSync sincroniza los datos de las aplicaciones Correo, Calendario y Contactos. Hemos agregado un motor de sincronización a la aplicación Outlook que proporciona la misma sincronización. |
| Compatibilidad con la API de compresión diferencial remota|La compatibilidad con la API de compresión diferencial remota permite sincronizar los datos con un origen remoto usando tecnologías de compresión que minimizan la cantidad de datos que se envían a través de la red. |
| Extensión de conmutador de filtro ligero de WFP|La extensión de conmutador de filtro ligero de WFP permite a los desarrolladores crear [extensiones de filtrado de paquetes de red sencillas para el conmutador virtual de Hyper-V](https://docs.microsoft.com/windows-hardware/drivers/network/using-virtual-switch-filtering). Puede lograr la misma funcionalidad creando una extensión de filtrado completa. Por lo tanto, retiraremos esta extensión en el futuro. |

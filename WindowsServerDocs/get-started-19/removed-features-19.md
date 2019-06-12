---
title: Características eliminadas o prevista su eliminación en Windows Server 2019
description: Obtenga información sobre las características y funcionalidades, quitado o planeado para la eliminación a partir de Windows Server 2019.
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
ms.openlocfilehash: 597da91aa40d9af4526b5358a88128b52d040645
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501440"
---
# <a name="features-removed-or-planned-for-replacement-starting-windows-server-2019"></a>Características eliminadas o planeado para el reemplazo a partir de Windows Server 2019

>Se aplica a: Windows Server 2019

Cada versión de Windows Server agrega nuevas características y funciones; ocasionalmente podemos eliminar características y funciones, normalmente debido a que hemos agregado una mejor opción. Estos son los detalles acerca de las características y funcionalidades que se quitaron en Windows Server 2019.

> [!TIP]
> - Puedes obtener acceso rápido a las versiones de Windows Server uniéndote al [Programa Windows Insider](https://insider.windows.com): esta es una excelente manera de probar los cambios de características.
> - ¿Tienes preguntas acerca de otras versiones? Consulte la información para [cuáles son las novedades en Windows Server](../get-started/whats-new-in-windows-server.md).

**La lista está sujeta a cambios y puede no incluir todas las características afectadas o funcionalidad.** 

## <a name="features-we-removed-in-this-release"></a>Características que hemos eliminado en esta versión

Nos estamos quitando las siguientes características y funcionalidades de la imagen de producto instalado en Windows Server 2019. Las aplicaciones o código que dependen de estas características no funcionarán en esta versión a menos que uses un método alternativo.

|Característica    |En su lugar, puedes usar...|
|-----------|--------------------
|Análisis de negocio, que también se denomina Administración de digitalización distribuida (DSM)|Nos estamos quitando este análisis segura y la capacidad de administración de analizador: no hay ningún dispositivo que admitan esta característica.|
|Imprimir componentes - componente ahora opcional para las instalaciones Server Core|En versiones anteriores de Windows Server, los componentes de impresión estaban *deshabilitado* de forma predeterminada en la opción de instalación Server Core. Se cambió en Windows Server 2016, lo que les permite de forma predeterminada. En Windows Server 2019, esos componentes de impresión una vez más están deshabilitadas de forma predeterminada para Server Core. Si tiene que habilitar los componentes de impresión, puede hacerlo ejecutando el **Install-WindowsFeature-servidor de impresión** cmdlet.|
|[Agente de conexión a Escritorio remoto y Host de virtualización de escritorio remoto](../remote/remote-desktop-services/desktop-hosting-service.md) en una instalación de Server Core|La mayoría de las implementaciones de servicios de escritorio remoto tienen estos roles colocalizados con el host de sesión de Escritorio remoto (RDSH), que requiere el Servidor con Experiencia de escritorio; para que sea coherente con RDSH vamos a cambiar estos roles para también requerir Servidor con Experiencia de escritorio. Estos roles RDS ya no están disponibles para su uso en un [instalación Server Core](../administration/server-core/what-is-server-core.md). Si necesita [implementar estas funciones como parte de su infraestructura de escritorio remoto](../remote/remote-desktop-services/rds-deploy-infrastructure.md), también puede [instalarlas en Windows Server con experiencia de escritorio](../get-started/getting-started-with-server-with-desktop-experience.md). <br/><br/>Estas funciones también se incluyen en la opción de instalación de Experiencia de escritorio de Windows Server 2019. |

## <a name="features-were-no-longer-developing"></a>Características que ya no se estamos desarrollando

Se están desarrollando activamente ya no estas características y puede quitarlos de una actualización futura. Algunas características se han reemplazado por otras características o funciones, mientras que otras están ahora disponibles de diferentes orígenes. 

Si tienes algún comentario acerca de la sustitución propuesta de cualquiera de estas características, puedes usar la [aplicación Centro de comentarios](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app). 

| Característica   | En su lugar, puedes usar... |
|-----------|---------------------|
| Unidad de almacenamiento de claves de Hyper-V|Ya no estamos trabajando en la característica de unidad de almacenamiento de la clave de Hyper-V. Si usa máquinas virtuales de generación 1, consulte [seguridad de virtualización de máquinas virtuales de generación 1](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/generation-1-virtual-machine-security-settings-for-hyper-v) para obtener información acerca de las opciones en el futuro. Si va a crear nuevas máquinas virtuales usar máquinas virtuales de generación 2 con dispositivos TPM para una solución más segura. |
| Consola de administración de confianza Platform Module (TPM)|La información que estaban disponible en la consola de administración de TPM está disponible en el [ **seguridad dispositivo** ](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/wdsc-device-security) página en el [Windows Defender Security Center](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/windows-defender-security-center). |
| Modo de atestación de Active Directory del servicio de protección de host|Modo de atestación del servicio de protección de Host Active Directory ya no estamos desarrollando: en su lugar, hemos agregado un nuevo modo de atestación [atestación de clave de host](../security/guarded-fabric-shielded-vm/guarded-fabric-create-host-key.md), que es mucho más sencillo e igualmente como compatible como basada en Active Directory atestación.  Este nuevo modo proporciona una funcionalidad equivalente con una experiencia de instalación, administración más simple y menos dependencias de infraestructura de la atestación de Active Directory. Atestación de clave de host no tiene ningún requisito de hardware adicional más allá de qué atestación de Active Directory es necesario, por lo que todos los sistemas existentes seguirán siendo compatibles con el nuevo modo. Consulte [implementar hosts protegidos](../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md) para obtener más información acerca de las opciones de atestación. |
| Servicio OneSync|El servicio OneSync sincroniza los datos para las aplicaciones de correo electrónico, calendario y las personas. Hemos agregado un motor de sincronización a la aplicación de Outlook que proporciona la sincronización misma. |
| Compatibilidad con las API de compresión diferencial remota|Compatibilidad con las API de compresión diferencial remota habilitada la sincronización de datos con un origen remoto mediante las tecnologías de compresión que minimizan la cantidad de datos que se envían a través de la red. |
| Extensión de conmutador de filtro ligero de WFP|La extensión de conmutador de filtro ligero de WFP permite a los desarrolladores crear [las extensiones para el conmutador virtual de Hyper-V de filtrado de paquetes de red simple](https://docs.microsoft.com/en-us/windows-hardware/drivers/network/using-virtual-switch-filtering). Puede lograr la misma funcionalidad mediante la creación de una extensión de filtrado completa. Por lo tanto, retiraremos esta extensión en el futuro. |

---
title: 'Windows Server, versión 1803: Características que se han quitado'
description: Obtén información acerca de las características eliminadas o en desuso en Windows Server, versión 1803 o una versión posterior
ms.mktglfcycl: plan
ms.localizationpriority: medium
ms.sitesec: library
author: jasongerend
ms.author: jgerend
ms.date: 10/22/2019
ms.topic: article
ms.openlocfilehash: 9483a0561091686f833875dc23cf895456f97d5e
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948051"
---
# <a name="features-removed-or-planned-for-replacement-starting-with-windows-server-version-1803"></a>Características eliminadas o que está previsto eliminar a partir de Windows Server, versión 1803

> Se aplica a: Windows Server, versión 1803

Cada versión de Windows Server agrega nuevas características y funciones. En ocasiones, también podemos eliminar características y funciones, normalmente debido a que hemos agregado una opción mejor. Estos son los detalles sobre las características y funciones que se han quitado en Windows Server, versión 1803.

> [!TIP]
> - Para obtener acceso rápido a las versiones de Windows Server, puedes unirte al [Programa Windows Insider](https://insider.windows.com). Esta es una excelente manera de probar los cambios de características.
> - ¿Tienes preguntas acerca de otras versiones? Consulta [Características eliminadas o cuyo reemplazo está planeado en Windows Server](../get-started-19/removed-features.md).

**La lista está sujeta a cambios y podría no incluir todas las características o funciones afectadas.**

## <a name="features-we-removed-in-this-release"></a>Características que hemos eliminado en esta versión

Hemos eliminado las siguientes características y funcionalidades de la imagen del producto instalado en Windows Server, versión 1803. Las aplicaciones o código que dependen de estas características no funcionarán en esta versión a menos que uses un método alternativo.

| Característica    | En su lugar, puedes usar... |
| ----------- | -------------------- |
| [Servicio de replicación de archivos](https://support.microsoft.com/help/4025991/windows-server-version-1709-no-longer-supports-frs)|Servicios de replicación de archivos, introducida en Windows Server 2003 R2, se ha sustituido por la replicación DFS. Es necesario [migrar cualquier controlador de dominio que use FRS a replicación DFS con SYSVOL](https://techcommunity.microsoft.com/t5/storage-at-microsoft/streamlined-migration-of-frs-to-dfsr-sysvol/ba-p/425405). |
| Virtualización de red de Hyper-V (HNV)|[Virtualización de la red](../networking/sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md) ahora se incluye en Windows Server como parte de la solución de [Redes definidas por software ](../networking/sdn/software-defined-networking.md) (SDN), que también incluye Controlador de red, Equilibrio de carga de software, Enrutamiento definido por el usuario y Listas de control de acceso. |

## <a name="features-were-no-longer-developing"></a>Características que ya no estamos desarrollando

Ya no desarrollamos activamente estas características y puede que se quiten en una futura actualización. Algunas características se han reemplazado por otras características o funciones, mientras que otras están ahora disponibles de diferentes orígenes.

>[!NOTE]
> Ten en cuenta que algunas de las características y funciones que se describen a continuación no se incluyen en la opción de instalación de Server Core, que se proporciona en Windows Server, versión 1803. *Están* presentes en el servidor con la opción de instalación de Experiencia de escritorio, que se publicó con Windows Server 2016 por última vez y se presentará de nuevo en Windows Server 2019.

Si tienes algún comentario acerca de la sustitución propuesta de cualquiera de estas características, puedes usar la [aplicación Centro de opiniones](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app).

| Característica o rol    | En su lugar, puedes usar... |
| ----------- | --------------------- |
| Análisis de negocio, que también se denomina Administración de digitalización distribuida (DSM)|La [funcionalidad de Administración de digitalización](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd759124\(v%3dws.11\)) se introdujo en Windows Server 2008 R2 y habilitó la digitalización segura y la administración de escáneres en una empresa. Ya no estamos invirtiendo en esta característica y no hay disponible ningún dispositivo que la admita. |
| Tecnologías de transición de IPv4/6 (6to4, ISATAP y Direct Tunnels)|6to4 está deshabilitada de forma predeterminada a partir de Windows 10, versión 1607 (la actualización de aniversario), ISATAP se ha deshabilitado de forma predeterminada a partir de Windows 10, versión 1703 (Creators Update), y Direct Tunnels siempre ha estado deshabilitado de forma predeterminada. Utiliza la compatibilidad con IPv6 nativo en su lugar. |
| [MultiPoint Services](../remote/multipoint-services/multipoint-services.md)|Ya no estamos desarrollando el rol MultiPoint Services como parte de Windows Server. Los servicios de MultiPoint Connector están disponibles a través de la [característica a petición](/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities) para Windows Server y Windows 10. Puedes usar [Servicios de Escritorio remoto](../remote/remote-desktop-services/welcome-to-rds.md), en concreto el host de sesión de Servicios de Escritorio remoto, para proporcionar conectividad con RDP. |
| [Paquetes de símbolos sin conexión](/windows-hardware/drivers/debugger/debugger-download-symbols) (MSI del símbolo de depuración)|Ya no estamos haciendo que los paquetes de símbolos estén disponibles como archivo MSI descargable. En su lugar, el [Servidor de símbolos de Microsoft está pasando a ser un almacén de símbolos basados en Azure](/archive/blogs/windbg/update-on-microsofts-symbol-server). Si necesitas los símbolos de Windows, conéctate al Servidor de símbolos de Microsoft para almacenar en caché los símbolos localmente o usa un archivo de manifiesto con SymChk.exe en un equipo con acceso a Internet. |
| [Agente de Conexión a Escritorio remoto y host de virtualización de Escritorio remoto](../remote/remote-desktop-services/desktop-hosting-service.md) en una instalación básica|La mayoría de las implementaciones de los Servicios de Escritorio remoto tienen estos roles colocalizados con el host de sesión de Escritorio remoto (RDSH), que requiere Windows Server con experiencia de escritorio. Para que sea coherente con RDSH vamos a cambiar estos roles para que también requieran Windows Server con experiencia de escritorio. Ya no estamos desarrollando estos roles de RDS para su uso en una [instalación básica](../administration/server-core/what-is-server-core.md). Si necesitas [implementar estos roles como parte de la infraestructura de Escritorio remoto](../remote/remote-desktop-services/rds-deploy-infrastructure.md), puedes [instalarlos en Windows Server 2016 con Experiencia de escritorio](getting-started-with-server-with-desktop-experience.md). <br/><br/>Estos roles también se incluyen en la opción de instalación de la experiencia de escritorio de Windows Server 2019. Puedes probarlos en la [versión Windows Insider de Windows Server 2019](/windows-insider/at-work/): asegúrate de elegir la imagen de LTSC. |
| [Adaptador de vídeo 3D RemoteFX (vGPU)](../virtualization/hyper-v/deploy/deploy-graphics-devices-using-remotefx-vgpu.md)|Estamos desarrollando nuevas opciones de aceleración de gráficos para entornos virtualizados. También puedes usar la [Asignación de dispositivos discreta (DDA)](../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md) como alternativa. |
| [Directivas de restricción de software](../identity/software-restriction-policies/software-restriction-policies.md) en Directiva de grupo|En lugar de usar las directivas de restricción de software a través de Directiva de grupo, puedes usar [AppLocker](/windows/security/threat-protection/applocker/applocker-overview) o [Control de aplicaciones de Windows Defender](/windows/security/threat-protection/windows-defender-application-control) para controlar las aplicaciones a las que los usuarios pueden acceder y el código que puede ejecutarse en el kernel. |
| Espacios de almacenamiento en una configuración compartida con un tejido de SAS|Implementa [espacios de almacenamiento directo](../storage/storage-spaces/storage-spaces-direct-overview.md) en su lugar. Espacios de almacenamiento directo admite el uso de caracteres de contenedores SAS con certificación HLK, pero en una configuración no compartida, tal como se describe en los [requisitos de hardware de espacios de almacenamiento directo](../storage/storage-spaces/storage-spaces-direct-hardware-requirements.md). |
| Experiencia con Windows Server Essentials|Ya no desarrollamos el rol de Experiencia de Windows Server Essentials para las SKU de Windows Server Standard o de Windows Server Datacenter. Si necesitas una solución de servidor fácil de usar para pequeñas y medianas empresas, comprueba nuestra nueva solución de [Microsoft 365 para la empresa](https://www.microsoft.com/microsoft-365/business) o usa [Windows Server 2016 Essentials](/windows-server-essentials/get-started/get-started). |
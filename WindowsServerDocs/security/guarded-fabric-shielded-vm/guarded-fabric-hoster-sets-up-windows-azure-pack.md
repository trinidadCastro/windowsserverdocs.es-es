---
title: 'Máquinas virtuales blindadas: el proveedor de servicios de hosting configura Microsoft Azure Pack'
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: d528c689-58b0-425c-9740-25e2553ed689
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 9ca9f41770c977b6e7c4900b090471dbfe11a450
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855726"
---
# <a name="shielded-vms---hosting-service-provider-sets-up-windows-azure-pack"></a>Máquinas virtuales blindadas: el proveedor de servicios de hosting configura Microsoft Azure Pack

Este tema describe cómo puede configurar un proveedor de hospedaje de servicio Windows Azure Pack para que los inquilinos pueden usar para implementar las máquinas virtuales blindadas. Paquete de Windows Azure es un portal web que extiende la funcionalidad de System Center Virtual Machine Manager para permitir que los inquilinos implementar y administrar sus propias máquinas virtuales a través de una sencilla interfaz web. Windows Azure Pack totalmente es compatible con las máquinas virtuales blindadas y facilita aún más para los inquilinos crear y administrar sus archivos de datos de blindaje.

Para entender cómo encaja este tema en el proceso general de la implementación de máquinas virtuales blindadas, consulte [hospeda los pasos de configuración del proveedor de servicio para hosts protegidos y máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="setting-up-windows-azure-pack"></a>Configurar Windows Azure Pack

Se realizarán las siguientes tareas para configurar Windows Azure Pack en su entorno:

1. Completar la configuración de System Center 2016 - Virtual Machine Manager (VMM) para el tejido de hospedaje. Esto incluye la configuración de plantillas de máquina virtual y una nube de máquinas virtuales que se expondrá a través de Windows Azure Pack:

    [Escenario: implementar hosts protegidos y máquinas virtuales blindadas en VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-overview)

2. Instale y configure System Center 2016 - Service Provider Foundation (SPF). Este software permite a Windows Azure Pack para comunicarse con los servidores VMM:

    [Implementar Service Provider Foundation - SPF](https://technet.microsoft.com/system-center-docs/spf/deploy/deploy-spf)

3. Instalar Windows Azure Pack y configurarla para comunicarse con SPF:

    - [Instalar Windows Azure Pack](#install-windows-azure-pack) (en este tema)
    - [Configurar Windows Azure Pack](#configure-windows-azure-pack) (en este tema)

4. Crear uno o varios planes de hospedaje en Windows Azure Pack para permitir que a los inquilinos acceso a las nubes de máquina virtual:

    [Crear un plan en Windows Azure Pack](#create-a-plan-in-windows-azure-pack) (en este tema)

## <a name="install-windows-azure-pack"></a>Instalar Windows Azure Pack

Instalar y configurar Windows Azure Pack (WAP) en el equipo donde desea hospedar el portal web para los inquilinos. Esta máquina debe ser capaz de alcanzar el servidor SPF y ser accesible por los inquilinos.

1.  Revisar [requisitos del sistema WAP](https://technet.microsoft.com/library/dn296442.aspx) e instale el [requisitos previos de software](https://technet.microsoft.com/library/dn469335.aspx).

2.  Descargue e instale el [instalador de plataforma Web](https://www.microsoft.com/web/downloads/platform.aspx). Si la máquina no está conectada a Internet, siga el [las instrucciones de instalación sin conexión](http://www.iis.net/learn/install/web-platform-installer/web-platform-installer-v4-command-line-webpicmdexe-rtw-release).

3.  Abra el instalador de plataforma Web y busque **Windows Azure Pack: Portal y API Express** bajo el **productos** ficha. Haga clic en **agregar**, a continuación, **instalar** en la parte inferior de la ventana.

4.  Lleve a cabo la instalación. Una vez finalizada la instalación, el sitio de configuración (*https://&lt;wapserver&gt;: 30101 /*) se abre en el explorador web. En este sitio Web, se proporciona información acerca de SQL server y finalizar la configuración de WAP.

Para Ayuda para instalar Windows Azure Pack, consulte [instalar una implementación rápida de Windows Azure Pack](https://technet.microsoft.com/dn296439.aspx).

> [!NOTE]
> Si ya ejecuta Windows Azure Pack en su entorno, puede usar la instalación existente. Para poder trabajar con la versión más reciente blindadas las características de la máquina virtual, sin embargo, debe actualizar su instalación en al menos Update Rollup 10.

### <a name="configure-windows-azure-pack"></a>Configurar Windows Azure Pack

Antes de usar Windows Azure Pack, debe tener ya se instaló y configuró para su infraestructura.

1.  Vaya al portal de administración de Windows Azure Pack en *https://&lt;wapserver&gt;: 30091*y, a continuación, inicie sesión con sus credenciales de administrador.

2.  En el panel izquierdo, haga clic en **nubes de máquinas virtuales**.

3.  Conectar Windows Azure Pack con la instancia de Service Provider Foundation haciendo **registrar System Center Service Provider Foundation**. Deberá especificar la dirección URL de Service Provider Foundation, así como un nombre de usuario y una contraseña.

    ![Registrar System Center Service Provider Foundation](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-01-register-spf.png)

4.  Una vez completado, podrá ver las nubes de máquina virtual configurado en el entorno de VMM. Asegúrese de que tener al menos una nube de máquinas virtuales que admite máquinas virtuales blindadas disponibles en WAP antes de continuar.

### <a name="create-a-plan-in-windows-azure-pack"></a>Crear un plan en Windows Azure Pack

Para permitir que los inquilinos crear máquinas virtuales de WAP, primero debe crear un plan de hospedaje para que los inquilinos puedan suscribirse. Los planes definen los permitidos nubes de máquinas virtuales, plantillas, redes y entidades de facturación para los inquilinos.

1.  En el panel inferior del portal, haga clic en **+ nuevo** &gt; **PLAN** &gt; **crear PLAN**.

2.  En el primer paso del asistente, elija un nombre para el Plan. Esto es el nombre de que los inquilinos verán al suscribirse.

3.  En el segundo paso, seleccione **NUBES de máquinas virtuales** como uno de los servicios que se va a ofrecer en el plan.

4.  Omita el paso acerca de cómo seleccionar los complementos para el plan.

5.  Haga clic en **Aceptar** (marca de verificación) para crear el plan. Aunque esto crea el plan, no está aún en un estado configurado.

    ![Los planes en Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-02-create-plan.png)

6.  Para empezar a configurar el Plan, haga clic en su nombre.

7.  En la página siguiente, en **servicios del plan**, haga clic en **nubes de máquinas virtuales**. Se abrirá la página donde puede configurar cuotas para este plan.

8.  En **básica**, seleccione el servidor de administración VMM y la nube de máquinas virtuales que desea ofrecer a los inquilinos. Se mostrarán las nubes que pueden ofrecer las máquinas virtuales blindadas con **(Blindaje compatible)** junto a su nombre.

9.  Seleccionar las cuotas que desea aplicar este plan. (Por ejemplo, los límites en el núcleo de CPU y uso de RAM). Asegúrese de dejar el **permitir que las máquinas virtuales para blindar** casilla activada.

    ![Configuración de nubes de máquinas virtuales en Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-03-virtual-machine-clouds.png)
    
10.  Desplácese hacia abajo hasta la sección titulada **plantillas**y, a continuación, seleccione una o varias plantillas para ofrecer a los inquilinos. Pueden ofrecer ambos blindada y se debe ofrecer no blindadas plantillas a los inquilinos, pero una plantilla blindada para proporcionar a los inquilinos-to-end garantías sobre la integridad de la máquina virtual y sus secretos.

11.  En el **redes** sección, agregue una o más redes para los inquilinos.

12.  Después de establecer cualquier otra configuración o las cuotas para el Plan, haga clic en **guardar** en la parte inferior.

13.  En la parte superior izquierda de la pantalla, haga clic en la flecha para ir a la **Plan** página.

14.  En la parte inferior de la pantalla, cambie el Plan se **privada** a **pública** para que los inquilinos puedan suscribirse al Plan.

    ![Cambiar el acceso para un plan en Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-04-change-access.png)

    En este momento, se configura Windows Azure Pack y los inquilinos podrán suscribirse al plan que acaba de crear e implementar máquinas virtuales blindadas. Para conocer pasos adicionales que debe completar los inquilinos, consulte [máquinas virtuales blindadas para inquilinos - implementación de una máquina virtual blindada con Windows Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md).

## <a name="see-also"></a>Vea también

- [Hospedaje de los pasos de configuración del proveedor de servicio para hosts protegidos y máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Las máquinas virtuales blindadas y tejido protegido](guarded-fabric-and-shielded-vms-top-node.md)

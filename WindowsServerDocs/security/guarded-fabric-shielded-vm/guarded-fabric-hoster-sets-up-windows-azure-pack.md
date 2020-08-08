---
title: 'Máquinas virtuales blindadas: el proveedor de servicios de hosting configura Microsoft Azure Pack'
ms.topic: article
ms.assetid: d528c689-58b0-425c-9740-25e2553ed689
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 3d6af6e6dea584485e2517d8e54c107c5cc2af90
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996267"
---
# <a name="shielded-vms---hosting-service-provider-sets-up-windows-azure-pack"></a>Máquinas virtuales blindadas: el proveedor de servicios de hosting configura Microsoft Azure Pack

En este tema se describe cómo un proveedor de servicios de hosting puede configurar Windows Azure Pack para que los inquilinos puedan usarlo para implementar máquinas virtuales blindadas. Windows Azure Pack es un portal web que amplía la funcionalidad de System Center Virtual Machine Manager para permitir que los inquilinos implementen y administren sus propias máquinas virtuales a través de una sencilla interfaz Web. Windows Azure Pack es totalmente compatible con máquinas virtuales blindadas y facilita aún más que los inquilinos creen y administren los archivos de datos de blindaje.

Para entender cómo se ajusta este tema en el proceso general de implementación de máquinas virtuales blindadas, consulte [los pasos de configuración de proveedor de servicio de hospedaje para hosts protegidos y máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="setting-up-windows-azure-pack"></a>Configuración de Windows Azure Pack

Realizará las tareas siguientes para configurar Windows Azure Pack en su entorno:

1. Complete la configuración de System Center 2016-Virtual Machine Manager (VMM) para el tejido de hospedaje. Esto incluye la configuración de plantillas de máquina virtual y una nube de máquinas virtuales, que se expondrán a través de Windows Azure Pack:

    [Escenario: Implementación de hosts protegidos y máquinas virtuales blindadas en VMM](/system-center/vmm/deploy-guarded-host-fabric?view=sc-vmm-2019)

2. Instale y configure System Center 2016-Service Provider Foundation (SPF). Este software permite a Windows Azure Pack comunicarse con los servidores VMM:

    [Implementación de Service Provider Foundation-SPF](https://technet.microsoft.com/system-center-docs/spf/deploy/deploy-spf)

3. Instale Windows Azure Pack y configúrelo para comunicarse con SPF:

    - [Instalar Windows Azure Pack](#install-windows-azure-pack) (en este tema)
    - [Configurar Windows Azure Pack](#configure-windows-azure-pack) (en este tema)

4. Cree uno o varios planes de hospedaje en Windows Azure Pack para permitir el acceso de los inquilinos a las nubes de máquinas virtuales:

    [Crear un plan en Windows Azure Pack](#create-a-plan-in-windows-azure-pack) (en este tema)

## <a name="install-windows-azure-pack"></a>Instalar Windows Azure Pack

Instale y configure Windows Azure Pack (WAP) en el equipo en el que desea hospedar el portal web para los inquilinos. Este equipo tendrá que ser capaz de acceder al servidor SPF y ser accesible para los inquilinos.

1.  Revisar [los requisitos del sistema WAP](/previous-versions/azure/windows-server-azure-pack/dn296442(v=technet.10)) e instalar el [software necesario](/previous-versions/azure/windows-server-azure-pack/dn469335(v=technet.10)).

2.  Descargue e instale el [instalador de plataforma web](https://www.microsoft.com/web/downloads/platform.aspx). Si el equipo no está conectado a Internet, siga las [instrucciones de instalación sin conexión](https://www.iis.net/learn/install/web-platform-installer/web-platform-installer-v4-command-line-webpicmdexe-rtw-release).

3.  Abra el instalador de plataforma web y busque **Windows Azure Pack: portal y API Express** en la pestaña **productos** . Haga clic en **Agregar**y luego en **instalar** en la parte inferior de la ventana.

4.  Lleve a cabo la instalación. Una vez finalizada la instalación, se abre el sitio de configuración (*https:// &lt; wapserver &gt; : 30101/*) en el explorador Web. En este sitio web, proporcione información sobre su SQL Server y termine de configurar WAP.

Para obtener ayuda para la configuración de Windows Azure Pack, consulte [instalar una implementación rápida de Windows Azure Pack](/previous-versions/azure/windows-server-azure-pack/dn296439(v=technet.10)).

> [!NOTE]
> Si ya ha ejecutado Windows Azure Pack en su entorno, puede usar la instalación existente. Sin embargo, para trabajar con las características más recientes de la máquina virtual blindada, deberá actualizar la instalación al menos con el paquete acumulativo de actualizaciones 10.

### <a name="configure-windows-azure-pack"></a>Configurar Windows Azure Pack

Antes de usar Windows Azure Pack, ya debe tener instalado y configurado para su infraestructura.

1.  Vaya al portal de administración de Windows Azure Pack en *https:// &lt; wapserver &gt; : 30091*y, a continuación, inicie sesión con sus credenciales de administrador.

2.  En el panel izquierdo, haga clic en **nubes de máquinas virtuales**.

3.  Para conectarse Windows Azure Pack a la instancia de Service Provider Foundation, haga clic en **registrar System Center Service Provider Foundation**. Tendrá que especificar la dirección URL de Service Provider Foundation, así como un nombre de usuario y una contraseña.

    ![Registrar System Center Service Provider Foundation](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-01-register-spf.png)

4.  Una vez completado, debería poder ver las nubes de máquinas virtuales configuradas en el entorno de VMM. Asegúrese de que tiene al menos una nube de máquinas virtuales que admite máquinas virtuales blindadas disponibles para WAP antes de continuar.

### <a name="create-a-plan-in-windows-azure-pack"></a>Crear un plan en Windows Azure Pack

Para permitir que los inquilinos creen máquinas virtuales en WAP, primero debe crear un plan de hospedaje al que los inquilinos puedan suscribirse. Los planes definen las nubes de máquinas virtuales permitidas, las plantillas, las redes y las entidades de facturación para los inquilinos.

1. En el panel inferior del portal, haga clic en **+ nuevo** &gt; **plan** &gt; **crear plan**.

2. En el primer paso del asistente, elija un nombre para el plan. Este es el nombre que verán los inquilinos cuando se suscriban.

3. En el segundo paso, seleccione **nubes de máquinas virtuales** como uno de los servicios que se van a ofrecer en el plan.

4. Omita el paso sobre cómo seleccionar los complementos para el plan.

5. Haga clic en **Aceptar** (marca de verificación) para crear el plan. Aunque esto crea el plan, aún no está en un estado configurado.

   ![Planes en Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-02-create-plan.png)

6. Para empezar a configurar el plan, haga clic en su nombre.

7. En la página siguiente, en **servicios del plan**, haga clic en **nubes de máquinas virtuales**. Se abrirá la página en la que puede configurar las cuotas de este plan.

8. En **básico**, seleccione el servidor de administración VMM y la nube de máquinas virtuales que desea ofrecer a los inquilinos. Las nubes que pueden ofrecer máquinas virtuales blindadas se mostrarán con **(se admite el blindaje)** junto a su nombre.

9. Seleccione las cuotas que desea aplicar a este plan. (Por ejemplo, límites en el uso de CPU y núcleos de CPU). Asegúrese de dejar activada la casilla **permitir que virtual machines esté blindada** .

   ![Configuración de nubes de máquinas virtuales en Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-03-virtual-machine-clouds.png)

10. Desplácese hacia abajo hasta la sección titulado **plantillas**y, a continuación, seleccione una o varias plantillas para su oferta a los inquilinos. Puede ofrecer plantillas blindadas y no blindadas a los inquilinos, pero se debe ofrecer una plantilla blindada para proporcionar a los inquilinos garantías completas sobre la integridad de la máquina virtual y sus secretos.

11. En la sección **redes** , agregue una o más redes para los inquilinos.

12. Después de establecer cualquier otra configuración o cuota del plan, haga clic en **Guardar** en la parte inferior.

13. En la parte superior izquierda de la pantalla, haga clic en la flecha para volver a la página **plan** .

14. En la parte inferior de la pantalla, cambie el plan de **privado** a **público** para que los inquilinos puedan suscribirse al plan.

    ![Cambiar el acceso de un plan en Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-04-change-access.png)

    En este momento, se configura Windows Azure Pack y los inquilinos podrán suscribirse al plan que acaba de crear e implementar máquinas virtuales blindadas. Para conocer los pasos adicionales que los inquilinos deben completar, consulte [máquinas virtuales blindadas para inquilinos: implementación de una máquina virtual blindada mediante Windows Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md).

## <a name="additional-references"></a>Referencias adicionales

- [Pasos de configuración del proveedor de servicios de hosting para hosts protegidos y máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [VM blindadas y tejido protegido](guarded-fabric-and-shielded-vms-top-node.md)
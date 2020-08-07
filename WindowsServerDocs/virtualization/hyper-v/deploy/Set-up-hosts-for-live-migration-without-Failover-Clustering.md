---
title: Configurar hosts para la migración en vivo sin clústeres de conmutación por error
description: Proporciona instrucciones para configurar la migración en vivo en un entorno no en clúster.
manager: dongill
ms.topic: article
ms.assetid: b5e3c405-cb76-4ff2-8042-c2284448c435
author: kbdazure
ms.author: kathydav
ms.date: 9/30/2016
ms.openlocfilehash: 7bcd4e625f340ba7358a8ce9bdd860581c390e96
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948024"
---
# <a name="set-up-hosts-for-live-migration-without-failover-clustering"></a>Configurar hosts para la migración en vivo sin clústeres de conmutación por error

>Se aplica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

En este artículo se muestra cómo configurar hosts que no están agrupados para que pueda realizar migraciones en vivo entre ellos. Siga estas instrucciones si no configuró la migración en vivo al instalar Hyper-V, o si desea cambiar la configuración. Para configurar los hosts en clúster, use herramientas para los clústeres de conmutación por error.

## <a name="requirements-for-setting-up-live-migration"></a>Requisitos para configurar la migración en vivo

Para configurar los hosts no en clúster para la migración en vivo, necesitará lo siguiente:

-  Una cuenta de usuario con permiso para realizar los distintos pasos. La pertenencia al grupo local Administradores de Hyper-V o al grupo administradores tanto en el equipo de origen como en el de destino cumple este requisito, a menos que esté configurando la delegación restringida. Se requiere la pertenencia al grupo administradores de dominio para configurar la delegación restringida.

- El rol Hyper-V en Windows Server 2016 o Windows Server 2012 R2 instalado en los servidores de origen y de destino. Puede realizar una migración en vivo entre los hosts que ejecutan Windows Server 2016 y Windows Server 2012 R2 si la máquina virtual es al menos la versión 5. <br>Para obtener instrucciones de actualización de la versión, consulte [actualización de la versión de la máquina virtual en Hyper-V en Windows 10 o Windows Server 2016](Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Para obtener instrucciones de instalación, consulte [instalar el rol Hyper-V en Windows Server](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).

- Equipos de origen y de destino que pertenezcan al mismo dominio Active Directory, o que pertenezcan a dominios que confían entre sí.
- Las herramientas de administración de Hyper-V instaladas en un equipo que ejecuta Windows Server 2016 o Windows 10, a menos que las herramientas estén instaladas en el servidor de origen o de destino, y que ejecute las herramientas desde el servidor.

## <a name="consider-options-for-authentication-and-networking"></a>Considerar las opciones de autenticación y redes

Tenga en cuenta cómo desea configurar lo siguiente:

-  **Autenticación**: ¿qué protocolo se usará para autenticar el tráfico de migración en vivo entre los servidores de origen y destino? La elección determina si necesitará iniciar sesión en el servidor de origen antes de iniciar una migración en vivo:
   - Kerberos le permite evitar tener que iniciar sesión en el servidor, pero requiere que se configure la delegación restringida. Consulte a continuación las instrucciones.
   - CredSSP le permite evitar la configuración de la delegación restringida, pero requiere que inicie sesión en el servidor de origen. Puede hacerlo a través de una sesión de consola local, una sesión de Escritorio remoto o una sesión remota de Windows PowerShell.

      CredSPP requiere iniciar sesión en situaciones que podrían no ser obvias. Por ejemplo, si inicia sesión en TestServer01 para trasladar una máquina virtual a TestServer02 y, después, desea devolver la máquina virtual a TestServer01, deberá iniciar sesión en TestServer02 antes de intentar volver a la máquina virtual a TestServer01. Si no lo hace, se producirá un error en el intento de autenticación, se producirá un error y se mostrará el siguiente mensaje:

      "Error en la operación de migración de la máquina virtual en el origen de la migración.
      No se pudo establecer una conexión con el *nombre del equipo*host: no hay credenciales disponibles en el paquete de seguridad 0x8009030E ".

-   **Rendimiento**: ¿tiene sentido configurar las opciones de rendimiento? Estas opciones pueden reducir el uso de la red y la CPU, además de agilizar las migraciones en vivo. Tenga en cuenta sus requisitos y su infraestructura, y Pruebe distintas configuraciones que le ayudarán a decidir. Las opciones se describen al final del paso 2.

-  **Preferencia de red**: ¿permitirá el tráfico de migración en vivo a través de cualquier red disponible o aislará el tráfico a redes específicas? Un procedimiento recomendado de seguridad es aislar el tráfico en redes privadas de confianza porque el tráfico de migración en vivo no se cifra cuando se envía por la red. El aislamiento de red se puede lograr a través de una red aislada físicamente o de otra tecnología de redes de confianza, como las redes VLAN.

## <a name="step-1-configure-constrained-delegation-optional"></a><a name="BKMK_Step1"></a>Paso 1: configuración de la delegación restringida (opcional)
Si ha decidido usar Kerberos para autenticar el tráfico de migración en vivo, configure la delegación restringida con una cuenta que sea miembro del grupo administradores de dominio.

### <a name="use-the-users-and-computers-snap-in-to-configure-constrained-delegation"></a>Usar el complemento usuarios y equipos para configurar la delegación restringida

1.  Abra el complemento Usuarios y equipos de Active Directory. (En Administrador del servidor, seleccione el servidor si no está seleccionado, haga clic en **herramientas**  >>  **Active Directory usuarios y equipos**).

2.  En el panel de navegación de **Active Directory usuarios y equipos**, seleccione el dominio y haga doble clic en la carpeta **equipos** .

3.  En la carpeta **equipos** , haga clic con el botón secundario en la cuenta de equipo del servidor de origen y, a continuación, haga clic en **propiedades**.

4.  En **propiedades**, haga clic en la pestaña **delegación** .

5.  En la pestaña delegación, seleccione **confiar en este equipo para la delegación solo a los servicios especificados** y, a continuación, seleccione **usar cualquier protocolo de autenticación**.

6.  Haga clic en **Agregar**.

7.  En **Agregar servicios**, haga clic en **usuarios o equipos**.

8.  En **Seleccionar usuarios o equipos**, escriba el nombre del servidor de destino. Haga clic en **Comprobar nombres** para comprobarlo y, a continuación, haga clic en **Aceptar**.

9. En **Agregar servicios**, en la lista de servicios disponibles, realice lo siguiente y, a continuación, haga clic en **Aceptar**:

    -   Para mover el almacenamiento de máquina virtual, seleccione **cifs**. Esto es necesario si desea trasladar el almacenamiento junto con la máquina virtual, así como si desea trasladar solo el almacenamiento de una máquina virtual. Si se configura el servidor para usar almacenamiento SMB para Hyper-V, este ya debe estar seleccionado.

    -   Para mover máquinas virtuales, seleccione **Servicio de migración del sistema virtual de Microsoft**.

10. En la pestaña **Delegación** del cuadro de diálogo Propiedades, compruebe que los servicios que seleccionó en el paso anterior se incluyan en lista como los servicios a los que el equipo de destino puede presentarle credenciales delegadas. Haga clic en **Aceptar**.

11. Desde la carpeta **Computers**, seleccione la cuenta de equipo del servidor de destino y repita el proceso. En el cuadro de diálogo **Seleccionar usuarios o equipos**, asegúrese de especificar el nombre del servidor de origen.

Los cambios de configuración surten efecto cuando se producen los siguientes pasos:

  -  Los cambios se replican en los controladores de dominio en los que están conectados los servidores que ejecutan Hyper-V.
  -  El controlador de dominio emite un nuevo vale de Kerberos.

## <a name="step-2-set-up-the-source-and-destination-computers-for-live-migration"></a><a name="BKMK_Step2"></a>Paso 2: configurar los equipos de origen y de destino para la migración en vivo
En este paso se incluye la elección de opciones de autenticación y redes. Como práctica recomendada de seguridad, se recomienda seleccionar redes específicas para usar para el tráfico de migración en vivo, como se explicó anteriormente. En este paso también se muestra cómo elegir la opción rendimiento.

### <a name="use-hyper-v-manager-to-set-up-the-source-and-destination-computers-for-live-migration"></a>Usar el administrador de Hyper-V para configurar los equipos de origen y de destino para la migración en vivo

1.  Abra el administrador de Hyper-V. (En Administrador del servidor, haga clic en **herramientas**  >> **Administrador de Hyper-V**).

2.  En el panel de navegación, seleccione uno de los servidores. (Si no aparece, haga clic con el botón secundario en **Administrador de Hyper-V**, haga clic en **conectar al servidor**, escriba el nombre del servidor y haga clic en **Aceptar**. Repita este procedimiento para agregar más servidores).

3.  En el panel **acción** , haga clic en **configuración de Hyper-V**  >> **migraciones en vivo**.

4.  En el panel **Migraciones en vivo**, active **Habilitar migraciones en vivo entrantes y salientes**.

5.  En **migraciones en vivo simultáneas**, especifique un número diferente si no desea usar el valor predeterminado de 2.

6.  En **Migraciones en vivo entrantes**, si quiere usar conexiones de red específicas para aceptar tráfico de migración en vivo, haga clic en **Agregar** para escribir la información de la dirección IP. De lo contrario, haga clic en **Usar cualquier red disponible para la migración en vivo**. Haga clic en **Aceptar**.

7.  Para elegir las opciones de Kerberos y rendimiento, expanda **migraciones en vivo** y, luego, seleccione **características avanzadas**.

    - Si ha configurado la delegación restringida, en **Protocolo de autenticación**, seleccione **Kerberos**.
    - En **Opciones de rendimiento**, revise los detalles y elija una opción diferente si es adecuado para su entorno.

8. Haga clic en **Aceptar**.

9. Seleccione el otro servidor en el administrador de Hyper-V y repita los pasos.

### <a name="use-windows-powershell-to-set-up-the-source-and-destination-computers-for-live-migration"></a>Usar Windows PowerShell para configurar los equipos de origen y de destino para la migración en vivo

Hay tres cmdlets disponibles para configurar la migración en vivo en hosts que no están en clúster: [enable-VMMigration](https://technet.microsoft.com/library/hh848544.aspx), [set-VMMigrationNetwork](https://technet.microsoft.com/library/hh848467.aspx)y [set-VMHost](https://technet.microsoft.com/library/hh848524.aspx). Este ejemplo utiliza los tres y hace lo siguiente:
  - Configura la migración en vivo en el host local
  - Permite el tráfico de migración entrante solo en una red específica
  - Elige Kerberos como el protocolo de autenticación

Cada línea representa un comando separado.

```PowerShell
PS C:\> Enable-VMMigration

PS C:\> Set-VMMigrationNetwork 192.168.10.1

PS C:\> Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos
```

Set-VMHost también le permite elegir una opción de rendimiento (y muchas otras opciones de configuración de host). Por ejemplo, para elegir SMB pero dejar el protocolo de autenticación establecido en el valor predeterminado de CredSSP, escriba:

```PowerShell
PS C:\> Set-VMHost -VirtualMachineMigrationPerformanceOption SMB
```

En esta tabla se describe cómo funcionan las opciones de rendimiento.

|Opción|Descripción|
|----------|---------------|
    |TCP/IP|Copia la memoria de la máquina virtual en el servidor de destino a través de una conexión TCP/IP.|
    |Compresión|Comprime el contenido de la memoria de la máquina virtual antes de copiarla en el servidor de destino a través de una conexión TCP/IP. **Nota:** Esta es la configuración **predeterminada** .|
    |SMB|Copia la memoria de la máquina virtual en el servidor de destino a través de una conexión SMB 3,0.<p>-SMB directo se usa cuando los adaptadores de red de los servidores de origen y de destino tienen habilitadas las funcionalidades de acceso directo a memoria remota (RDMA).<br />-SMB multicanal detecta y usa automáticamente varias conexiones cuando se identifica una configuración de SMB multicanal adecuada.<p>Para obtener más información, vea [Mejorar el rendimiento de un servidor de archivos con SMB directo](https://technet.microsoft.com/library/jj134210(WS.11).aspx).|

 ## <a name="next-steps"></a>Pasos siguientes

 Después de configurar los hosts, está listo para realizar una migración en vivo. Para obtener instrucciones, consulte [uso de la migración en vivo sin clústeres de conmutación por error para migrar una máquina virtual](../manage/Use-live-migration-without-Failover-Clustering-to-move-a-virtual-machine.md).

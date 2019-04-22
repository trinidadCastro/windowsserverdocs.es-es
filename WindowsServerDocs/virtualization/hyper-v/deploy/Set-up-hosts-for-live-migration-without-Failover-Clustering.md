---
title: Configuración de hosts para migración en vivo sin clústeres de conmutación por error
description: Proporciona instrucciones sobre cómo configurar la migración en vivo en un entorno no agrupado
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5e3c405-cb76-4ff2-8042-c2284448c435
author: KBDAzure
ms.author: kathydav
ms.date: 9/30/2016
ms.openlocfilehash: 49e36d7fae5eec07772fd6f82c5a5d69838351d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821996"
---
# <a name="set-up-hosts-for-live-migration-without-failover-clustering"></a>Configuración de hosts para migración en vivo sin clústeres de conmutación por error

>Se aplica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server, Microsoft Hyper-V Server 2019 de 2019

Este artículo muestra cómo configurar los hosts que no están agrupados por lo que puede migraciones en vivo entre ellos. Siga estas instrucciones si no ha configurado la migración en vivo al instalar Hyper-V, o si desea cambiar la configuración. Para configurar hosts en clúster, use las herramientas de agrupación en clústeres de conmutación por error.  
  
## <a name="requirements-for-setting-up-live-migration"></a>Requisitos para configurar la migración en vivo  
  
Para configurar los hosts no en clúster para la migración en vivo, necesitará:   
  
-  Una cuenta de usuario con permiso para realizar los distintos pasos. Pertenencia al grupo local Administradores de Hyper-V o del grupo Administradores en equipos de origen y destino cumple este requisito, a menos que se va a configurar la delegación restringida. Pertenencia al grupo Administradores de dominio es necesario para configurar la delegación restringida.  
  
- El rol de Hyper-V en Windows Server 2016 o Windows Server 2012 R2 instalado en los servidores de origen y destino. Puede hacer una migración en vivo entre hosts que ejecutan Windows Server 2016 y Windows Server 2012 R2, si la máquina virtual sea al menos la versión 5. <br>Para obtener instrucciones de actualización de versión, consulte [versión actualización máquina virtual de Hyper-V en Windows 10 o Windows Server 2016](Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Para obtener instrucciones de instalación, consulte [instalar el rol de Hyper-V en Windows Server](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).  
  
- Equipos de origen y destino que pertenecen al mismo dominio de Active Directory, o que pertenecen a dominios que confían entre sí.    
- Las herramientas de administración de Hyper-V instaladas en un equipo que ejecuta Windows Server 2016 o Windows 10, a menos que las herramientas se instalan en el servidor de origen o destino y podrán ejecutar las herramientas desde el servidor.  
  
## <a name="consider-options-for-authentication-and-networking"></a>Considere la posibilidad de red y autenticación  
  
Tenga en cuenta cómo desea configurar lo siguiente:  
  
-  **Autenticación**: ¿El protocolo que se usará para autenticar el tráfico de migración en vivo entre los servidores de origen y destino? La elección determina si tiene que iniciar sesión el servidor de origen antes de iniciar una migración en vivo:   
   - Kerberos le permite evitar tener que iniciar sesión en el servidor, pero requiere configurar la delegación restringida. Para obtener instrucciones, consulte a continuación.  
   - CredSSP permite evitar configurar la delegación restringida, pero requiere que inicie sesión en el servidor de origen. Puede hacerlo a través de una sesión de consola local, una sesión de escritorio remoto o una sesión remota de Windows PowerShell.  
  
      CredSPP requiere iniciar sesión para las situaciones que podrían no ser obvias. Por ejemplo, si inicia sesión TestServer01 para mover una máquina virtual a TestServer02 y, a continuación, va a mover la máquina virtual de regreso a TestServer01, deberá iniciar sesión en TestServer02 antes de intentar mover la máquina virtual de regreso a TestServer01. Si no lo hace, el intento de autenticación se produce un error, se produce un error, y se muestra el mensaje siguiente:  
    
      "Error de operación de migración de máquina Virtual en el origen de la migración.  
      No se pudo establecer una conexión con el host *nombre_equipo*: No hay credenciales están disponibles en el paquete de seguridad 0x8009030E".
  
-   **Rendimiento**: ¿Tiene sentido para configurar las opciones de rendimiento? Estas opciones pueden reducir el uso de CPU y red, así como agilizar las migraciones en vivo. Tenga en cuenta sus requisitos y la infraestructura y probar diferentes configuraciones para ayudarle a decidir. Al final del paso 2 se describen las opciones.  
  
-  **Preferencias de red**: ¿Permitirá tráfico de migración en vivo a través de alguna red activa o aislará el tráfico a redes específicas? Un procedimiento recomendado de seguridad es aislar el tráfico en redes privadas de confianza porque el tráfico de migración en vivo no se cifra cuando se envía por la red. El aislamiento de red se puede lograr a través de una red aislada físicamente o de otra tecnología de redes de confianza, como las redes VLAN.  
  
## <a name="BKMK_Step1"></a>Paso 1: Configurar la delegación restringida (opcional)  
Si ha decidido usar Kerberos para autenticar el tráfico de migración en vivo, configurar la delegación restringida con una cuenta que sea miembro del grupo Administradores de dominio.  
  
### <a name="use-the-users-and-computers-snap-in-to-configure-constrained-delegation"></a>Use el complemento Usuarios y equipos para configurar la delegación restringida  
  
1.  Abra el complemento Usuarios y equipos de Active Directory. (Desde el administrador del servidor, seleccione el servidor si no está seleccionada, haga clic en **herramientas** >> **equipos y usuarios de Active Directory**).  
  
2.  En el panel de navegación en **equipos y usuarios de Active Directory**, seleccione el dominio y haga doble clic en el **equipos** carpeta.  
  
3.  Desde el **equipos** carpeta, haga clic en la cuenta de equipo del servidor de origen y, a continuación, haga clic en **propiedades**.  
  
4.  Desde **propiedades**, haga clic en el **delegación** ficha.  
  
5.  En la ficha de delegación, seleccione **confiar en este equipo para la delegación a los servicios especificados sólo** y, a continuación, seleccione **usar cualquier protocolo de autenticación**.  
  
6.  Haz clic en **Agregar**.  
  
7.  Desde **agregar servicios**, haga clic en **usuarios o equipos**.  
  
8.  Desde **Seleccionar usuarios o equipos**, escriba el nombre del servidor de destino. Haga clic en **comprobar nombres** para comprobarlo y, a continuación, haga clic en **Aceptar**.  
  
9. Desde **agregar servicios**, en la lista de servicios disponibles, haga lo siguiente y, a continuación, haga clic en **Aceptar**:  
  
    -   Para mover el almacenamiento de máquina virtual, seleccione **cifs**. Esto es necesario si desea mover el almacenamiento junto con la máquina virtual, así como si desea mover solo un almacenamiento de máquina virtual. Si se configura el servidor para usar almacenamiento SMB para Hyper-V, este ya debe estar seleccionado.  
  
    -   Para mover máquinas virtuales, seleccione **Servicio de migración del sistema virtual de Microsoft**.  
  
10. En la pestaña **Delegación** del cuadro de diálogo Propiedades, compruebe que los servicios que seleccionó en el paso anterior se incluyan en lista como los servicios a los que el equipo de destino puede presentarle credenciales delegadas. Haga clic en **Aceptar**.  
  
11. Desde la carpeta **Computers** , seleccione la cuenta de equipo del servidor de destino y repita el proceso. En el cuadro de diálogo **Seleccionar usuarios o equipos**, asegúrese de especificar el nombre del servidor de origen.  
  
Los cambios de configuración surtan efecto, que se hayan producen por las dos acciones siguientes:  
  
  -  Los cambios se replican en los controladores de dominio que los servidores que ejecutan Hyper-V se registran en.  
  -  El controlador de dominio emite un nuevo vale de Kerberos.  
  
## <a name="BKMK_Step2"></a>Paso 2: Configurar los equipos de origen y destino para la migración en vivo  
Este paso incluye la elección de las opciones para la autenticación y las redes. Como práctica recomendada de seguridad, se recomienda seleccionar redes específicas que se usarán para el tráfico de migración en vivo, según se explicó anteriormente. Este paso también muestra cómo elegir la opción de rendimiento.   
  
### <a name="use-hyper-v-manager-to-set-up-the-source-and-destination-computers-for-live-migration"></a>Utilice el Administrador de Hyper-V para configurar los equipos de origen y destino para la migración en vivo  
  
1.  Abre el Administrador Hyper-V. (Desde el administrador del servidor, haga clic en **herramientas** >>**Administrador de Hyper-V**.)  
  
2.  En el panel de navegación, seleccione uno de los servidores. (Si no aparece, haga clic en **Administrador de Hyper-V**, haga clic en **conectar al servidor**, escriba el nombre del servidor y haga clic en **Aceptar**. Repita el proceso para agregar más servidores.)  
  
3.  En el **acción** panel, haga clic en **configuración de Hyper-V** >>**migraciones en vivo**.  
  
4.  En el panel **Migraciones en vivo** , active **Habilitar migraciones en vivo entrantes y salientes**.  
  
5.  En **migraciones en vivo simultáneas**, especifique un número diferente si no desea utilizar el valor predeterminado de 2.  
  
6.  En **Migraciones en vivo entrantes**, si quiere usar conexiones de red específicas para aceptar tráfico de migración en vivo, haga clic en **Agregar** para escribir la información de la dirección IP. De lo contrario, haga clic en **Usar cualquier red disponible para la migración en vivo**. Haga clic en **Aceptar**.  
  
7.  Para elegir las opciones de Kerberos y el rendimiento, expanda **migraciones en vivo** y, a continuación, seleccione **características avanzadas**.  
  
    - Si ha configurado la delegación restringida en **protocolo de autenticación**, seleccione **Kerberos**.  
    - En **las opciones de rendimiento**, revise los detalles y elija una opción diferente si es adecuado para su entorno.   
  
8. Haga clic en **Aceptar**.  
  
9. Seleccione otro servidor en el Administrador de Hyper-V y repita los pasos.  
  
### <a name="use-windows-powershell-to-set-up-the-source-and-destination-computers-for-live-migration"></a>Usar Windows PowerShell para configurar los equipos de origen y destino para la migración en vivo  
  
Tres cmdlets están disponibles para la configuración de migración en vivo en hosts no en clúster: [Enable-VMMigration](https://technet.microsoft.com/library/hh848544.aspx), [Set-VMMigrationNetwork](https://technet.microsoft.com/library/hh848467.aspx), y [Set-VMHost](https://technet.microsoft.com/library/hh848524.aspx). Este ejemplo utiliza las tres y hace lo siguiente:   
  - Configura la migración en vivo en el host local  
  - Permite el tráfico de migración entrante solamente en una red específica  
  - Elige Kerberos como el protocolo de autenticación   
  
Cada línea representa un comando separado.  
  
```  
PS C:\> Enable-VMMigration  
  
PS C:\> Set-VMMigrationNetwork 192.168.10.1  
  
PS C:\> Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos  
  
```  
Set-VMHost también le permite elegir una opción de rendimiento (y muchas otras opciones de host). Por ejemplo, para elegir SMB, pero deje el protocolo de autenticación con el valor predeterminado de CredSSP, escriba:  
  
```  
PS C:\> Set-VMHost -VirtualMachineMigrationPerformanceOption SMBTransport  
  
```  
  
Esta tabla describe cómo funcionan las opciones de rendimiento.  
  
|Opción|Descripción|  
|----------|---------------|  
    |TCP/IP|Copia la memoria de la máquina virtual en el servidor de destino a través de una conexión TCP/IP.|  
    |Compresión|Comprime el contenido de la memoria de la máquina virtual antes de copiarlo en el servidor de destino a través de una conexión TCP/IP. **Nota:** Se trata de la **predeterminada** configuración.|  
    |SMB|Copia la memoria de la máquina virtual en el servidor de destino a través de una conexión SMB 3.0.<br /><br />-SMB directo se usa cuando los adaptadores de red en los servidores de origen y destino tienen capacidades de acceso de memoria directa remota (RDMA) habilitadas.<br />-SMB multicanal detecta automáticamente y usa varias conexiones cuando se identifica una configuración de SMB multicanal adecuada.<br /><br />Para obtener más información, consulte [Improve Performance of a File Server with SMB Direct](https://technet.microsoft.com/library/jj134210(WS.11).aspx).|  
      
 ## <a name="next-steps"></a>Pasos siguientes
 
 Después de configurar los hosts, está listo para realizar una migración en vivo. Para obtener instrucciones, consulte [usar migración en vivo sin clústeres de conmutación por error para mover una máquina virtual](../manage/Use-live-migration-without-Failover-Clustering-to-move-a-virtual-machine.md). 
    



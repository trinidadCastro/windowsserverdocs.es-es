---
title: PASO 6 instalar y configurar 2-DC1
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar una implementación de multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d66901a-c40b-474c-9948-f989f399cfea
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8c3e12f9d64619ce00e578c7a8096c764e68ff7d
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283157"
---
# <a name="step-6-install-and-configure-2-dc1"></a>PASO 6 instalar y configurar 2-DC1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

2-DC1 proporciona los siguientes servicios:  
  
-   Un controlador de dominio para el dominio de Active Directory Domain Services (AD DS) corp2.corp.contoso.com.  
  
-   Un servidor DNS para el dominio DNS corp2.corp.contoso.com.  
  
Configuración de 2-DC1 consta de las siguientes acciones:  
  
- Instalar el sistema operativo en 2-DC1
  
- Configurar las propiedades TCP/IP

- Configuración de 2-DC1 como un controlador de dominio y servidor DNS

- Otorgamiento de permisos de directiva de grupo a CORP\User1
  
- Permitir que los equipos CORP2 obtener certificados de equipo
  
- Forzar la replicación entre DC1 y 2-DC1
  
## <a name="install-the-operating-system-on-2-dc1"></a>Instalar el sistema operativo en 2-DC1  
En primer lugar, instale Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
### <a name="to-install-the-operating-system-on-2-dc1"></a>Para instalar el sistema operativo en 2-DC1  
  
1.  Inicie la instalación de Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
2.  Siga las instrucciones para completar la instalación, especificando Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (instalación completa) y una contraseña segura para la cuenta de administrador local. Inicie sesión con la cuenta Administrador local.  
  
3.  2-DC1 de conectarse a una red que tenga acceso a Internet y ejecute Windows Update para instalar las actualizaciones más recientes para Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 y, a continuación, desconectarse de Internet.  
  
4.  2-DC1 se conecte a la subred Corpnet de 2.  
  
## <a name="configure-tcpip-properties"></a>Configurar las propiedades TCP/IP  
Configure el protocolo TCP/IP con direcciones IP estáticas.  
  
### <a name="to-configure-tcpip-on-2-dc1"></a>Para configurar TCP/IP en DC1 de 2  
  
1.  En la consola de administrador del servidor, haga clic en **servidor Local**y, a continuación, en el **propiedades** área, junto a **conexión cableada Ethernet**, haga clic en el vínculo.  
  
2.  En **Conexiones de red**, haga clic con el botón secundario en **Conexión cableada Ethernet** y, a continuación, haga clic en **Propiedades**.  
  
3.  Haga clic en **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, en **Propiedades**.  
  
4.  Haga clic en **Usar la siguiente dirección IP**. En **dirección IP**, tipo **10.2.0.1**. En **Máscara de subred**, escriba **255.255.255.0**. En **puerta de enlace predeterminada**, tipo **10.2.0.254**. Haga clic en **usar las siguientes direcciones de servidor DNS**, en **servidor DNS preferido**, tipo **10.2.0.1**y en **servidor DNS alternativo**, tipo **10.0.0.1**.  
  
5.  Haga clic en **Opciones avanzadas** y, a continuación, haga clic en la pestaña **DNS**.  
  
6.  En **sufijo DNS para esta conexión**, tipo **corp2.corp.contoso.com**y, a continuación, haga clic en **Aceptar** dos veces.  
  
7.  Haga clic en **Protocolo de Internet versión 6 (TCP/IPv6)** y, a continuación, haga clic en **Propiedades**.  
  
8.  Haga clic en **usar la siguiente dirección IPv6**. En **dirección IPv6**, tipo **2001:db8:2::1**. En **longitud del prefijo de subred**, tipo **64**. En **puerta de enlace predeterminada**, tipo **2001:db8:2::fe**. Haga clic en **usar las siguientes direcciones de servidor DNS**, en **servidor DNS preferido**, tipo **2001:db8:2::1**y en **servidor DNS alternativo**, tipo **2001:db8:1::1**.  
  
9. Haga clic en **Opciones avanzadas** y, a continuación, haga clic en la pestaña **DNS**.  
  
10. En **sufijo DNS para esta conexión**, tipo **corp2.corp.contoso.com**y, a continuación, haga clic en **Aceptar** dos veces.  
  
11. En el **propiedades de conexión cableada Ethernet** cuadro de diálogo, haga clic en **cerrar**.  
  
12. Cierre la ventana **Conexiones de red**.  
  
13. En la consola de administrador del servidor, en **servidor Local**, en el **propiedades** área, junto a **nombre_equipo**, haga clic en el vínculo.  
  
14. En el cuadro de diálogo **Propiedades del sistema**, en la pestaña **Nombre de equipo** haz clic en **Cambiar**.  
  
15. En el **cambios de dominio o el nombre de equipo** cuadro de diálogo **nombre_equipo**, tipo **2-DC1**y, a continuación, haga clic en **Aceptar**.  
  
16. Cuando se le indique que debe reiniciar el equipo, haga clic en **Aceptar**.  
  
17. En el cuadro de diálogo **Propiedades del sistema** , haga clic en **Cerrar**.  
  
18. Cuando se le pida que reinicie el equipo, haga clic en **Reiniciar ahora**.  
  
19. Después de reiniciar, inicie sesión con la cuenta de administrador local.  
  
## <a name="configure-2-dc1-as-a-domain-controller-and-dns-server"></a>Configuración de 2-DC1 como un controlador de dominio y servidor DNS  
Configure 2-DC1 como un controlador de dominio para el dominio corp2.corp.contoso.com y como un servidor DNS para el dominio DNS corp2.corp.contoso.com.  
  
### <a name="to-configure-2-dc1-as-a-domain-controller-and-dns-server"></a>Configuración de 2-DC1 como un controlador de dominio y servidor DNS  
  
1.  En la consola de administrador del servidor, en el **panel**, haga clic en **agregar roles y características**.  
  
2.  Haga clic en **siguiente** tres veces para ir a la pantalla de selección de roles de servidor  
  
3.  En el **seleccionar roles de servidor** , seleccione **Active Directory Domain Services**. Haga clic en **agregar características** cuando se le pida y, a continuación, haga clic en **siguiente** tres veces.  
  
4.  En la página **Confirmar selecciones de instalación** , haga clic en **Instalar**.  
  
5.  Cuando la instalación finalice correctamente, haga clic en **promover este servidor a controlador de dominio**.  
  
6.  En el Asistente de configuración de Active Directory Domain Services, en el **configuración de implementación** página, haga clic en **agregar un nuevo dominio a un bosque existente**.  
  
7.  En **nombre del dominio primario**, tipo **corp.contoso.com**, en **nuevo nombre de dominio**, tipo **corp2**.  
  
8.  En **proporcionar las credenciales para realizar esta operación**, haga clic en **cambio**. En el **Windows Security** cuadro de diálogo **nombre de usuario**, tipo **corp.contoso.com\Administrator**y en **contraseña**, escriba el corp\ Contraseña de administrador, haga clic en **Aceptar**y, a continuación, haga clic en **siguiente**.  
  
9. En el **opciones del controlador de dominio** página, asegúrese de que el **nombre del sitio** es **segundo sitio**. En **escriba la contraseña del modo de restauración de servicios de directorio (DSRM)** , en **contraseña** y **Confirmar contraseña**, escriba una contraseña segura dos veces y, a continuación, haga clic en  **Siguiente** cinco veces.  
  
10. En el **comprobación de requisitos previos** página después de que se validen los requisitos previos, haga clic en **instalar**.  
  
11. Espere a que la configuración de servicios de Active Directory y DNS complete el asistente y, a continuación, haga clic en **cerrar**.  
  
12. Una vez reiniciado el equipo, inicie sesión en el dominio CORP2 con la cuenta de administrador.  
  
## <a name="provide-group-policy-permissions-to-corpuser1"></a>Otorgamiento de permisos de directiva de grupo a CORP\User1  
Utilice este procedimiento para proporcionar al usuario de CORP\User1 con todos los permisos para crear y modificar objetos de directiva de grupo corp2.  
  
### <a name="to-provide-group-policy-permissions"></a>Para proporcionar los permisos de directiva de grupo  
  
1.  En el **iniciar** , escriba**gpmc.msc**, y, a continuación, presione ENTRAR.  
  
2.  En la consola de administración de directivas de grupo, abra **bosque: corp.contoso.com/Domains/corp2.corp.contoso.com**.  
  
3.  En el panel de detalles, haga clic en el **delegación** ficha. En el **permiso** la lista desplegable, haga clic en **vincular GPO**.  
  
4.  Haga clic en **agregar**y en el nuevo **Seleccionar usuario, equipo o grupo** cuadro de diálogo, haga clic en **ubicaciones**.  
  
5.  En el **ubicaciones** cuadro de diálogo el **ubicación** de árbol, haga clic en **corp.contoso.com**y haga clic en **Aceptar**.  
  
6.  En **escriba el nombre de objeto a seleccionar** tipo **User1**, haga clic en **Aceptar**y en el **Agregar grupo o usuario** cuadro de diálogo, haga clic en **Aceptar** .  
  
7.  En la consola de administración de directivas de grupo, en el árbol, haga clic en **Group Policy Objects**, y en los detalles del panel, haga clic en el **delegación** ficha.  
  
8.  Haga clic en **agregar**y en el nuevo **Seleccionar usuario, equipo o grupo** cuadro de diálogo, haga clic en **ubicaciones**.  
  
9. En el **ubicaciones** cuadro de diálogo el **ubicación** de árbol, haga clic en **corp.contoso.com**y haga clic en **Aceptar**.  
  
10. En **escriba el nombre de objeto a seleccionar** tipo **User1**, haga clic en **Aceptar**.  
  
11. En la consola de administración de directivas de grupo, en el árbol, haga clic en **filtros WMI**, y en los detalles del panel, haga clic en el **delegación** ficha.  
  
12. Haga clic en **agregar**y en el nuevo **Seleccionar usuario, equipo o grupo** cuadro de diálogo, haga clic en **ubicaciones**.  
  
13. En el **ubicaciones** cuadro de diálogo el **ubicación** de árbol, haga clic en **corp.contoso.com**y haga clic en **Aceptar**.  
  
14. En **escriba el nombre de objeto a seleccionar** tipo **User1**, haga clic en **Aceptar**. En el **Agregar grupo o usuario** diálogo cuadro, asegúrese de que **permisos** se establecen en **control total**y, a continuación, haga clic en **Aceptar**.  
  
15. Cierre la consola de Administración de directivas de grupo.  
  
## <a name="allow-corp2-computers-to-obtain-computer-certificates"></a>Permitir que los equipos CORP2 obtener certificados de equipo 

Equipos del dominio CORP2 deben obtener certificados de equipo de la entidad de certificación en APP1. Realice este procedimiento en APP1.  
  
### <a name="to-allow-corp2-computers-to-automatically-obtain-computer-certificates"></a>Para permitir que los equipos CORP2 obtener automáticamente certificados de equipo  
  
1.  En APP1, haga clic en **iniciar**, tipo **certtmpl.msc**, y, a continuación, presione ENTRAR.  
  
2.  En el **consola de plantillas de certificados**, en el panel central, haga doble clic en **autenticación cliente-servidor**.  
  
3.  En el **las propiedades de autenticación de cliente / servidor** cuadro de diálogo, haga clic en el **seguridad** ficha.  
  
4.  Haga clic en **agregar**y en el **Seleccionar usuarios, equipos, cuentas de servicio o grupos** cuadro de diálogo, haga clic en **ubicaciones**.  
  
5.  En el **ubicaciones** cuadro de diálogo **ubicación**, expanda **corp.contoso.com**, haga clic en **corp2.corp.contoso.com**y, a continuación, haga clic en  **Aceptar**.  
  
6.  En **escriba los nombres de objeto para seleccionar**, tipo **Admins. del dominio; Los equipos del dominio** y, a continuación, haga clic en **Aceptar**.  
  
7.  En el **las propiedades de autenticación de cliente / servidor** cuadro de diálogo **los nombres de usuario o grupo**, haga clic en **Admins. del dominio (administradores CORP2\Domain)** y en  **Permisos para que los administradores de dominio**, en el **permitir** columna, seleccione **escribir** y **inscribir**.  
  
8.  En **los nombres de usuario o grupo**, haga clic en **equipos del dominio (equipos CORP2\Domain)** y en **permisos para los equipos del dominio**, en la **permitir**columna, seleccione **inscribir** y **inscripción automática**y, a continuación, haga clic en **Aceptar**.  
  
9. Cierra la Consola de plantillas de certificado.  
  
## <a name="replication"></a>Forzar la replicación entre DC1 y 2-DC1  
Para poder inscribir certificados en 2 EDGE1, debe forzar la replicación de la configuración de DC1 a 2-DC1. Esta operación debe realizarse en DC1.  
  
### <a name="to-force-replication"></a>Para forzar la replicación  
  
1.  En DC1, haga clic en **iniciar**y, a continuación, haga clic en **servicios y sitios de Active Directory**.  
  
2.  En la consola de servicios y sitios de Active Directory, en el árbol, expanda **transportes entre sitios**y, a continuación, haga clic en **IP**.  
  
3.  En el panel de detalles, haga doble clic en **DEFAULTIPSITELINK**.  
  
4.  En el **DEFAULTIPSITELINK propiedades** cuadro de diálogo **costo**, tipo **1**, en **replicar cada**, tipo **15**y, a continuación, haga clic en **Aceptar**. Espere 15 minutos para que finalice la replicación.  
  
5.  Para forzar la replicación ahora en el árbol de consola, expanda **configuración Web\Sitio-First-Site-name\Servers\DC1\NTDS**, en el panel de detalles, haga clic en **<automatically generated>** , haga clic en  **Replicar ahora**y, a continuación, en el **Replicar ahora** cuadro de diálogo, haga clic en **Aceptar**.  
  
6.  Para asegurarse de que se ha completado correctamente la replicación haga lo siguiente:  
  
    1.  En el **iniciar** , escriba**cmd.exe**, y, a continuación, presione ENTRAR.  
  
    2.  Escriba el siguiente comando y presione ENTRAR.  
  
        ```  
        repadmin /syncall /e /A /P /d /q  
        ```  
  
    3.  Asegúrese de que todas las particiones se sincronizan sin errores. Si no es así, a continuación, vuelva a ejecutar el comando hasta que se notifica ningún error antes de continuar.  
  
7.  Cierre la ventana del símbolo del sistema.  
  



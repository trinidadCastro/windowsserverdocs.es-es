---
title: 'Paso 6: instalación y configuración de 2-DC1'
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de una implementación multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d66901a-c40b-474c-9948-f989f399cfea
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4c7a8243922f58f9705a85cd30b2a68cf4d876c6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404776"
---
# <a name="step-6-install-and-configure-2-dc1"></a>Paso 6: instalación y configuración de 2-DC1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

2-DC1 proporciona los siguientes servicios:  
  
-   Un controlador de dominio para el dominio corp2.corp.contoso.com Active Directory Domain Services (AD DS).  
  
-   Un servidor DNS para el dominio DNS de corp2.corp.contoso.com.  
  
la configuración de 2 a DC1 consta de lo siguiente:  
  
- Instalar el sistema operativo en 2-DC1
  
- Configurar las propiedades TCP/IP

- Configurar 2-DC1 como controlador de dominio y servidor DNS

- Proporcionar permisos de directiva de grupo a CORP\User1
  
- Permitir a los equipos CORP2 obtener certificados de equipo
  
- Forzar la replicación entre DC1 y 2-DC1
  
## <a name="install-the-operating-system-on-2-dc1"></a>Instalar el sistema operativo en 2-DC1  
En primer lugar, instale Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
### <a name="to-install-the-operating-system-on-2-dc1"></a>Para instalar el sistema operativo en 2-DC1  
  
1.  Inicie la instalación de Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
2.  Siga las instrucciones para completar la instalación, especificando Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (instalación completa) y una contraseña segura para la cuenta de administrador local. Inicie sesión con la cuenta Administrador local.  
  
3.  Conecte 2-DC1 a una red que tenga acceso a Internet y ejecute Windows Update para instalar las actualizaciones más recientes para Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 y, a continuación, desconéctese de Internet.  
  
4.  Conecte 2-DC1 a la subred 2-CorpNet.  
  
## <a name="configure-tcpip-properties"></a>Configurar las propiedades TCP/IP  
Configure el protocolo TCP/IP con direcciones IP estáticas.  
  
### <a name="to-configure-tcpip-on-2-dc1"></a>Para configurar TCP/IP en 2-DC1  
  
1.  En la consola de Administrador del servidor, haga clic en **servidor local**y, a continuación, en el área **propiedades** , junto a **conexión cableada Ethernet**, haga clic en el vínculo.  
  
2.  En **Conexiones de red**, haga clic con el botón secundario en **Conexión cableada Ethernet** y, a continuación, haga clic en **Propiedades**.  
  
3.  Haga clic en **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, en **Propiedades**.  
  
4.  Haga clic en **Usar la siguiente dirección IP**. En **dirección IP**, escriba **10.2.0.1**. En **Máscara de subred**, escriba **255.255.255.0**. En **puerta de enlace predeterminada**, escriba **10.2.0.254**. Haga clic en **usar las siguientes direcciones de servidor DNS**, en **servidor DNS preferido**, escriba **10.2.0.1**y, en **servidor DNS alternativo**, escriba **10.0.0.1**.  
  
5.  Haga clic en **Opciones avanzadas** y, a continuación, haga clic en la pestaña **DNS**.  
  
6.  En **sufijo DNS para esta conexión**, escriba **Corp2.Corp.contoso.com**y, a continuación, haga clic en **Aceptar** dos veces.  
  
7.  Haga clic en **Protocolo de Internet versión 6 (TCP/IPv6)** y, a continuación, haga clic en **Propiedades**.  
  
8.  Haga clic en **usar la siguiente dirección IPv6**. En **dirección IPv6**, escriba **2001: db8:2:: 1**. En **longitud del prefijo de subred**, escriba **64**. En **puerta de enlace predeterminada**, escriba **2001: db8:2:: fe**. Haga clic en **usar las siguientes direcciones de servidor DNS**, en **servidor DNS preferido**, escriba **2001: db8:2:: 1**y, en **servidor DNS alternativo**, escriba **2001: db8:1:: 1**.  
  
9. Haga clic en **Opciones avanzadas** y, a continuación, haga clic en la pestaña **DNS**.  
  
10. En **sufijo DNS para esta conexión**, escriba **Corp2.Corp.contoso.com**y, a continuación, haga clic en **Aceptar** dos veces.  
  
11. En el cuadro de diálogo **propiedades de conexión cableada Ethernet** , haga clic en **cerrar**.  
  
12. Cierre la ventana **Conexiones de red**.  
  
13. En la consola de Administrador del servidor, en **servidor local**, en el área **propiedades** , junto a **nombre de equipo**, haga clic en el vínculo.  
  
14. En el cuadro de diálogo **Propiedades del sistema**, en la pestaña **Nombre de equipo** haz clic en **Cambiar**.  
  
15. En el cuadro de diálogo cambios en el **dominio o el nombre del equipo** , en **nombre de equipo**, escriba **2-DC1**y, a continuación, haga clic en **Aceptar**.  
  
16. Cuando se le indique que debe reiniciar el equipo, haga clic en **Aceptar**.  
  
17. En el cuadro de diálogo **Propiedades del sistema** , haga clic en **Cerrar**.  
  
18. Cuando se le pida que reinicie el equipo, haga clic en **Reiniciar ahora**.  
  
19. Después de reiniciar, inicie sesión con la cuenta de administrador local.  
  
## <a name="configure-2-dc1-as-a-domain-controller-and-dns-server"></a>Configurar 2-DC1 como controlador de dominio y servidor DNS  
Configure 2-DC1 como controlador de dominio para el dominio corp2.corp.contoso.com y como servidor DNS para el dominio DNS corp2.corp.contoso.com.  
  
### <a name="to-configure-2-dc1-as-a-domain-controller-and-dns-server"></a>Para configurar 2-DC1 como controlador de dominio y servidor DNS  
  
1.  En la consola de Administrador del servidor, en el **Panel**, haga clic en **Agregar roles y características**.  
  
2.  Haga clic en **siguiente** tres veces para ir a la pantalla de selección de roles de servidor  
  
3.  En la página **Seleccionar roles de servidor** , seleccione **Active Directory Domain Services**. Haga clic en **Agregar características** cuando se le pida y, a continuación, haga clic en **siguiente** tres veces.  
  
4.  En la página **Confirmar selecciones de instalación** , haga clic en **Instalar**.  
  
5.  Cuando la instalación se complete correctamente, haga clic en **promover este servidor a controlador de dominio**.  
  
6.  En el Asistente para configuración de Active Directory Domain Services, en la página **configuración de implementación** , haga clic en **Agregar un nuevo dominio a un bosque existente**.  
  
7.  En **nombre de dominio primario**, escriba **Corp.contoso.com**, en **nuevo nombre de dominio**, escriba **Corp2**.  
  
8.  En **proporcione las credenciales para realizar esta operación**, haga clic en **cambiar**. En el cuadro de diálogo **seguridad de Windows** , en **nombre de usuario**, escriba **Corp. contoso. com\Administrator**y, en **contraseña**, escriba la contraseña de corp\administrador, haga clic en **Aceptar**y, a continuación, haga clic en **siguiente**.  
  
9. En la página **Opciones del controlador de dominio** , asegúrese de que el nombre del **sitio** sea el **segundo sitio**. En **Escriba la contraseña de modo de restauración de servicios de directorio (DSRM)** , en **contraseña** y **Confirmar contraseña**, escriba una contraseña segura dos veces y, a continuación, haga clic en **siguiente** cinco veces.  
  
10. En la página **comprobación de requisitos previos** , una vez validados los requisitos previos, haga clic en **instalar**.  
  
11. Espere hasta que el asistente complete la configuración de Active Directory y los servicios DNS y, a continuación, haga clic en **cerrar**.  
  
12. Una vez reiniciado el equipo, inicie sesión en el dominio CORP2 con la cuenta de administrador.  
  
## <a name="provide-group-policy-permissions-to-corpuser1"></a>Proporcionar permisos de directiva de grupo a CORP\User1  
Utilice este procedimiento para proporcionar al usuario CORP\User1 permisos completos para crear y cambiar objetos de directiva de grupo de Corp2.  
  
### <a name="to-provide-group-policy-permissions"></a>Para proporcionar permisos de directiva de grupo  
  
1.  En la pantalla **Inicio** , escriba**GPMC. msc**y, a continuación, presione Entrar.  
  
2.  En la consola de administración de directiva de grupo, Abra **Forest: Corp.contoso.com/Domains/Corp2.Corp.contoso.com**.  
  
3.  En el panel de detalles, haga clic en la pestaña **delegación** . En la lista desplegable **permiso** , haga clic en **vincular GPO**.  
  
4.  Haga clic en **Agregar**y, en el cuadro de diálogo **Seleccionar usuarios, equipos o grupos** , haga clic en **ubicaciones**.  
  
5.  En el cuadro de diálogo **ubicaciones** , en el árbol **Ubicación** , haga clic en **Corp.contoso.com**y, a continuación, haga clic en **Aceptar**.  
  
6.  En **Escriba el nombre del objeto para seleccionar el** tipo **user1**, haga clic en **Aceptar**y, en el cuadro de diálogo **Agregar grupo o usuario** , haga clic en **Aceptar**.  
  
7.  En la consola de administración de directiva de grupo, en el árbol, haga clic en **Directiva de grupo objetos**y, en el panel de detalles, haga clic en la pestaña **delegación** .  
  
8.  Haga clic en **Agregar**y, en el cuadro de diálogo **Seleccionar usuarios, equipos o grupos** , haga clic en **ubicaciones**.  
  
9. En el cuadro de diálogo **ubicaciones** , en el árbol **Ubicación** , haga clic en **Corp.contoso.com**y, a continuación, haga clic en **Aceptar**.  
  
10. En **Escriba el nombre del objeto para seleccionar el** tipo **user1**, haga clic en **Aceptar**.  
  
11. En la consola de administración de directiva de grupo, en el árbol, haga clic en **filtros WMI**y, en el panel de detalles, haga clic en la pestaña **delegación** .  
  
12. Haga clic en **Agregar**y, en el cuadro de diálogo **Seleccionar usuarios, equipos o grupos** , haga clic en **ubicaciones**.  
  
13. En el cuadro de diálogo **ubicaciones** , en el árbol **Ubicación** , haga clic en **Corp.contoso.com**y, a continuación, haga clic en **Aceptar**.  
  
14. En **Escriba el nombre del objeto para seleccionar el** tipo **user1**, haga clic en **Aceptar**. En el cuadro de diálogo **Agregar grupo o usuario** , asegúrese de que **los permisos** estén establecidos en **control total**y, a continuación, haga clic en **Aceptar**.  
  
15. Cierre la consola de Administración de directivas de grupo.  
  
## <a name="allow-corp2-computers-to-obtain-computer-certificates"></a>Permitir a los equipos CORP2 obtener certificados de equipo 

Los equipos del dominio CORP2 deben obtener certificados de equipo de la entidad de certificación en APP1. Realice este procedimiento en APP1.  
  
### <a name="to-allow-corp2-computers-to-automatically-obtain-computer-certificates"></a>Para permitir que los equipos CORP2 obtengan automáticamente certificados de equipo  
  
1.  En APP1, haga clic en **Inicio**, escriba **certtmpl. msc**y, a continuación, presione Entrar.  
  
2.  En la **consola de plantillas de certificados**, en el panel central, haga doble clic en **autenticación de cliente y servidor**.  
  
3.  En el cuadro de diálogo **propiedades de autenticación cliente-servidor** , haga clic en la pestaña **seguridad** .  
  
4.  Haga clic en **Agregar**y, en el cuadro de diálogo **Seleccionar usuarios, equipos, cuentas de servicio o grupos** , haga clic en **ubicaciones**.  
  
5.  En el cuadro de diálogo **ubicaciones** , en **Ubicación**, expanda **corp.contoso.com**, haga clic en **Corp2.Corp.contoso.com**y, a continuación, haga clic en **Aceptar**.  
  
6.  En **Escriba los nombres de objeto que desea seleccionar**, escriba **Admins. del dominio; Equipos del dominio** y, a continuación, haga clic en **Aceptar**.  
  
7.  En el cuadro de diálogo **propiedades de autenticación cliente-servidor** , en **nombres de grupos o usuarios**, haga clic en **Admins. del dominio (administradores de CORP2\Domain)** y, en **permisos de administradores de dominio**, en la columna **permitir** , seleccione **escribir** . e **inscribirse**.  
  
8.  En **nombres de grupos o usuarios**, haga clic en **equipos del dominio (equipos CORP2\Domain)** y, en **permisos de equipos del dominio**, en la columna **permitir** , seleccione **inscribir** e **inscripción automática**y, a continuación, haga clic en **Aceptar**.  
  
9. Cierra la Consola de plantillas de certificado.  
  
## <a name="replication"></a>Forzar la replicación entre DC1 y 2-DC1  
Para poder inscribirse en los certificados en 2-EDGE1, debe forzar la replicación de la configuración de DC1 a 2-DC1. Esta operación debe realizarse en DC1.  
  
### <a name="to-force-replication"></a>Para forzar la replicación  
  
1.  En DC1, haga clic en **Inicio**y, a continuación, haga clic en **sitios y servicios de Active Directory**.  
  
2.  En la consola sitios y servicios de Active Directory, en el árbol, expanda **transportes entre sitios**y, a continuación, haga clic en **IP**.  
  
3.  En el panel de detalles, haga doble clic en **DEFAULTIPSITELINK**.  
  
4.  En el cuadro de diálogo **propiedades de DEFAULTIPSITELINK** , en **costo**, escriba **1**, en **replicar cada**, escriba **15**y, a continuación, haga clic en **Aceptar**. Espere 15 minutos para que se complete la replicación.  
  
5.  Para forzar la replicación ahora en el árbol de consola, expanda **configuración de Sites\Default-First-Site-name\Servers\DC1\NTDS**, en el panel de detalles, haga clic con el botón secundario en **<automatically generated>** , haga clic en **Replicar ahora**y, a continuación, en el cuadro de diálogo **Replicar ahora** , Haga clic en **Aceptar**.  
  
6.  Para asegurarse de que la replicación se ha completado correctamente, haga lo siguiente:  
  
    1.  En la pantalla **Inicio** , escriba**cmd. exe**y, a continuación, presione Entrar.  
  
    2.  Escriba el siguiente comando y presione ENTRAR.  
  
        ```  
        repadmin /syncall /e /A /P /d /q  
        ```  
  
    3.  Asegúrese de que todas las particiones estén sincronizadas sin errores. Si no es así, vuelva a ejecutar el comando hasta que no se notifique ningún error antes de continuar.  
  
7.  Cierre la ventana del símbolo del sistema.  
  



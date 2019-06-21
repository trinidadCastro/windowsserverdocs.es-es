---
title: Unión a dominio sin conexión de DirectAccess
description: Esta guía explica los pasos para realizar una unión a un dominio sin conexión con DirectAccess en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 55528736-6c19-40bd-99e8-5668169ef3c7
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 59b5933a81c7021e58ea14e6ea4c4da374ce35cb
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283662"
---
# <a name="directaccess-offline-domain-join"></a>Unión a dominio sin conexión de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Esta guía explica los pasos para realizar una unión a un dominio sin conexión con DirectAccess. Durante una unión a un dominio sin conexión, un equipo está configurado para unirse a un dominio sin conexión VPN o física.  
  
Esta guía incluye las siguientes secciones:  
  
- Información general de unión de dominio sin conexión  
  
- Requisitos para unirse a un dominio sin conexión
  
- Proceso de unión al dominio sin conexión
  
- Pasos para realizar una unión a un dominio sin conexión  
  
## <a name="offline-domain-join-overview"></a>Información general de unión de dominio sin conexión  
Se introdujo en Windows Server 2008 R2, los controladores de dominio incluyen una característica llamada la unión a un dominio sin conexión. Una utilidad de línea de comandos denominada Djoin.exe le permite unir un equipo a un dominio sin físicamente al completar la operación de unión de dominio, póngase en contacto con un controlador de dominio. Los pasos generales para usar Djoin.exe son:  
  
1.  Ejecute **djoin /provision** para crear los metadatos de la cuenta de equipo. El resultado de este comando es un archivo .txt que incluye un blob codificado en base 64.  
  
2.  Ejecute **djoin /requestODJ** para insertar los metadatos de la cuenta de equipo desde el archivo .txt en el directorio de Windows del equipo de destino.  
  
3.  Reinicie el equipo de destino y el equipo se unirá al dominio.  
  
### <a name="BKMK_ODJOverview"></a>Unión a un dominio sin conexión con información general del escenario de las directivas de DirectAccess  
Unión a dominio sin conexión de DirectAccess es un proceso que los equipos que ejecutan Windows Server 2016, Windows Server 2012, Windows 10 y Windows 8 pueden usar para unirse a un dominio sin que se está combinando físicamente a la red corporativa o a través de VPN. Esto hace posible unir equipos a un dominio desde ubicaciones donde no hay ninguna conectividad a una red corporativa. Unión a un dominio sin conexión para DirectAccess proporciona directivas de DirectAccess a los clientes para permitir el aprovisionamiento remoto.  
  
Unirse a un dominio crea una cuenta de equipo y establece una relación de confianza entre un equipo que ejecuta un sistema operativo de Windows y un dominio de Active Directory.  
  
## <a name="BKMK_ODJRequirements"></a>Preparar para la unión a un dominio sin conexión  
  
1.  Cree la cuenta de equipo.  
  
2.  Inventario de la pertenencia de todos los grupos de seguridad al que pertenece la cuenta de equipo.  
  
3.  Recopile los certificados de equipo obligatorio, las directivas de grupo y objetos de directiva de grupo se aplique a los clientes nuevos.  
  
. Las siguientes secciones explican los requisitos del sistema operativo y requisitos de credenciales para realizar una combinación de dominio sin conexión de DirectAccess usar Djoin.exe.  
  
### <a name="operating-system-requirements"></a>Requisitos de sistema operativo  
Puede ejecutar Djoin.exe para DirectAccess únicamente en equipos que ejecutan Windows Server 2016, Windows Server 2012 o Windows 8. El equipo en el que se ejecuta Djoin.exe para aprovisionar datos de la cuenta de equipo en AD DS debe estar ejecutando Windows Server 2016, Windows 10, Windows Server 2012 o Windows 8. El equipo que desea unir al dominio también debe ejecutar Windows Server 2016, Windows 10, Windows Server 2012 o Windows 8.  
  
### <a name="credential-requirements"></a>Requisitos de credenciales  
Para realizar una unión a un dominio sin conexión, debe tener los derechos necesarios unir las estaciones de trabajo al dominio. Los miembros del grupo Admins. del dominio tienen estos derechos de forma predeterminada. Si no es un miembro del grupo Admins. del dominio, debe completar un miembro del grupo Admins. del dominio de una de las siguientes acciones para poder unir las estaciones de trabajo al dominio:  
  
-   Usar la directiva de grupo para conceder los derechos de usuario necesarios. Este método permite crear equipos en el contenedor equipos predeterminado y en cualquier unidad organizativa (OU) que se crea más adelante (si no hay entradas de control de acceso (ACE) de denegación se agregan).  
  
-   Editar la lista de control de acceso (ACL) del contenedor equipos predeterminado para el dominio delegar los permisos correctos para usted.  
  
-   Crear una unidad organizativa y editar la ACL en esa unidad organizativa para que le conceda el **crear secundaria - permitir** permiso. Pase el **/machineOU** parámetro para el **djoin /provision** comando.  
  
Los procedimientos siguientes muestran cómo conceder los derechos de usuario con la directiva de grupo y cómo delegar los permisos correctos.  
  
#### <a name="granting-user-rights-to-join-workstations-to-the-domain"></a>Conceder derechos de usuario a una de las estaciones de trabajo al dominio  
Puede usar la consola de administración de directivas de grupo (GPMC) para modificar la directiva de dominio o crear una nueva directiva que tiene una configuración que concede derechos de usuario para agregar estaciones de trabajo a un dominio.  
  
Pertenencia a **Admins. del dominio**, o equivalente, es lo mínimo necesario para conceder derechos de usuario.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477) (https://go.microsoft.com/fwlink/?LinkId=83477).   
  
###### <a name="to-grant-rights-to-join-workstations-to-a-domain"></a>Para conceder derechos para unir las estaciones de trabajo a un dominio  
  
1.  Haga clic en **Inicio**, en **Herramientas administrativas** y, a continuación, en **Administración de directivas de grupo**.  
  
2.  Haga doble clic en el nombre del bosque, haga doble clic en **dominios**, haga doble clic en el nombre del dominio en el que desea unir un equipo, haga clic en **Default Domain Policy**y, a continuación, haga clic en  **Editar**.  
  
3.  En el árbol de consola, haga doble clic en **configuración del equipo**, haga doble clic en **directivas**, haga doble clic en **Windows configuración**, haga doble clic en **seguridad Configuración de**, haga doble clic en **directivas locales**y, a continuación, haga doble clic en **asignación de derechos de usuario**.  
  
4.  En el panel de detalles, haga doble clic en **agregar estaciones de trabajo al dominio**.  
  
5.  Seleccione el **definir esta configuración de directiva** casilla de verificación y, a continuación, haga clic en **Agregar usuario o grupo**.  
  
6.  Escriba el nombre de la cuenta que desea conceder los derechos de usuario a y, a continuación, haga clic en **Aceptar** dos veces.  
  
## <a name="BKMK_ODKSxS"></a>Proceso de unión al dominio sin conexión  
Ejecute Djoin.exe en un símbolo del sistema con privilegios elevados para aprovisionar los metadatos de la cuenta de equipo. Al ejecutar el comando de aprovisionamiento, los metadatos de la cuenta de equipo se crean en un archivo binario que especifica como parte del comando.  
  
Para obtener más información acerca de la función NetProvisionComputerAccount que se usa para aprovisionar la cuenta de equipo durante una unión a un dominio sin conexión, consulte [NetProvisionComputerAccount función](https://go.microsoft.com/fwlink/?LinkId=162426) (https://go.microsoft.com/fwlink/?LinkId=162426). Para obtener más información acerca de la función NetRequestOfflineDomainJoin que se ejecuta localmente en el equipo de destino, vea [NetRequestOfflineDomainJoin función](https://go.microsoft.com/fwlink/?LinkId=162427) (https://go.microsoft.com/fwlink/?LinkId=162427).  
  
## <a name="BKMK_ODJSteps"></a>Pasos para realizar una unión a dominio sin conexión de DirectAccess  
El proceso de unión de dominio sin conexión incluye los pasos siguientes:  
  
1.  Crear una nueva cuenta de equipo para cada uno de los clientes remotos y generar un paquete de aprovisionamiento mediante el comando Djoin.exe desde un ya dominio unido el equipo en la red corporativa.  
  
2.  Agregar el equipo cliente al grupo de seguridad DirectAccessClients  
  
3.  Transferir el paquete de aprovisionamiento de forma segura a los equipos remotos (s) que se va a unir el dominio.  
  
4.  Aplique el paquete de aprovisionamiento y unir al cliente al dominio.  
  
5.  Reinicie el cliente para completar la unión al dominio y establecer la conectividad.  
  
Hay dos opciones a tener en cuenta al crear el paquete de aprovisionamiento para el cliente. Si usa al Asistente para introducción para instalar DirectAccess sin PKI, a continuación, debe usar la opción 1, a continuación. Si usa al Asistente para configuración avanzada para instalar DirectAccess con la PKI, a continuación, debe usar la opción 2 a continuación.  
  
Complete los pasos siguientes para realizar la unión al dominio sin conexión:  
  
##### <a name="option1-create-a-provisioning-package-for-the-client-without-pki"></a>Opción 1: Crear un paquete de aprovisionamiento para el cliente sin la PKI  
  
1.  En un símbolo del sistema del servidor de acceso remoto, escriba el siguiente comando para aprovisionar la cuenta de equipo:  
  
    ```  
    Djoin /provision /domain <your domain name> /machine <remote machine name> /policynames DA Client GPO name /rootcacerts /savefile c:\files\provision.txt /reuse  
    ```  
  
##### <a name="option2-create-a-provisioning-package-for-the-client-with-pki"></a>Opción2: Crear un paquete de aprovisionamiento para el cliente con la PKI  
  
1.  En un símbolo del sistema del servidor de acceso remoto, escriba el siguiente comando para aprovisionar la cuenta de equipo:  
  
    ```  
    Djoin /provision /machine <remote machine name> /domain <Your Domain name> /policynames <DA Client GPO name> /certtemplate <Name of client computer cert template> /savefile c:\files\provision.txt /reuse  
    ```  
  
##### <a name="add-the-client-computer-to-the-directaccessclients-security-group"></a>Agregar el equipo cliente al grupo de seguridad DirectAccessClients  
  
1.  En el controlador de dominio de **iniciar** , escriba **Active** y seleccione **equipos y usuarios de Active Directory** desde **aplicaciones** pantalla .  
  
2.  Expanda el árbol bajo el dominio y seleccione el **usuarios** contenedor.  
  
3.  En el panel de detalles, haga clic en **DirectAccessClients**y haga clic en **propiedades**.  
  
4.  En la pestaña **Miembros** , haga clic en **Agregar**.  
  
5.  Haga clic en **Tipos de objeto**, seleccione **Equipos** y, a continuación, haga clic en **Aceptar**.  
  
6.  Escriba el nombre del cliente para agregar y, a continuación, haga clic en **Aceptar**.  
  
7.  Haga clic en **Aceptar** para cerrar el **DirectAccessClients** cuadro de diálogo Propiedades y, a continuación, cierre **equipos y usuarios de Active Directory**.  
  
##### <a name="copy-and-then-apply-the-provisioning-package-to-the-client-computer"></a>Copiar y, a continuación, aplicar el paquete de aprovisionamiento en el equipo cliente  
  
1.  Copie el paquete de aprovisionamiento de c:\files\provision.txt en el servidor de acceso remoto, donde se ha guardado, c:\provision\provision.txt en el equipo cliente.  
  
2.  En el equipo cliente, abra un símbolo del sistema con privilegios elevados y, a continuación, escriba el comando siguiente para solicitar la unión al dominio:  
  
    ```  
    Djoin /requestodj /loadfile C:\provision\provision.txt /windowspath %windir% /localos  
    ```  
  
3.  Reinicie el equipo cliente. El equipo se unirá al dominio. Después del reinicio, el cliente se unirá al dominio y tiene conectividad a la red corporativa mediante DirectAccess.  
  
## <a name="see-also"></a>Vea también  
[Función NetProvisionComputerAccount](https://go.microsoft.com/fwlink/?LinkId=162426)  
[Función NetRequestOfflineDomainJoin](https://go.microsoft.com/fwlink/?LinkId=162427)  
  



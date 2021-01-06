---
title: Unión a dominio sin conexión de DirectAccess
description: En esta guía se explican los pasos para realizar una Unión a un dominio sin conexión con DirectAccess en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 55528736-6c19-40bd-99e8-5668169ef3c7
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: d18d643d6c614a90f4c9ab43b59c0b8777b2f6e8
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946791"
---
# <a name="directaccess-offline-domain-join"></a>Unión a dominio sin conexión de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En esta guía se explican los pasos para realizar una Unión a un dominio sin conexión con DirectAccess. Durante una Unión a un dominio sin conexión, se configura un equipo para unirse a un dominio sin conexión física o VPN.

Esta guía incluye las siguientes secciones:

- Introducción a la Unión a un dominio sin conexión

- Requisitos para la Unión a un dominio sin conexión

- Proceso de unión a dominio sin conexión

- Pasos para realizar una Unión a un dominio sin conexión

## <a name="offline-domain-join-overview"></a>Introducción a la Unión a un dominio sin conexión
En Windows Server 2008 R2, los controladores de dominio incluyen una característica denominada Unión a un dominio sin conexión. Una utilidad de línea de comandos denominada Djoin.exe permite unir un equipo a un dominio sin tener que ponerse en contacto físicamente con un controlador de dominio mientras se completa la operación de unión al dominio. Los pasos generales para utilizar Djoin.exe son:

1.  Ejecute **djoin/provision** para crear los metadatos de la cuenta de equipo. La salida de este comando es un archivo. txt que incluye un BLOB codificado en base 64.

2.  Ejecute **djoin/requestODJ** para insertar los metadatos de la cuenta de equipo del archivo. txt en el directorio de Windows del equipo de destino.

3.  Reinicie el equipo de destino y el equipo se unirá al dominio.

### <a name="offline-domain-join-with-directaccess-policies-scenario-overview"></a><a name="BKMK_ODJOverview"></a>Información general del escenario de unión a dominio sin conexión con directivas de DirectAccess
La Unión a un dominio sin conexión de DirectAccess es un proceso por el que los equipos que ejecutan Windows Server 2016, Windows Server 2012, Windows 10 y Windows 8 pueden usar para unirse a un dominio sin estar Unidos físicamente a la red corporativa ni conectarse a través de VPN. Esto permite unir equipos a un dominio desde ubicaciones en las que no hay conectividad a una red corporativa. Unión a un dominio sin conexión para DirectAccess proporciona directivas de DirectAccess a los clientes para permitir el aprovisionamiento remoto.

Una Unión a un dominio crea una cuenta de equipo y establece una relación de confianza entre un equipo que ejecuta un sistema operativo Windows y un dominio de Active Directory.

## <a name="prepare-for-offline-domain-join"></a><a name="BKMK_ODJRequirements"></a>Preparación para la Unión a un dominio sin conexión

1.  Cree la cuenta de la máquina.

2.  Inventariar la pertenencia de todos los grupos de seguridad a los que pertenece la cuenta de equipo.

3.  Recopile los certificados de equipo, las directivas de grupo y los objetos de directiva de grupo necesarios para aplicarlos a los nuevos clientes.

. En las siguientes secciones se explican los requisitos del sistema operativo y los requisitos de credenciales para realizar una Unión de dominio sin conexión de DirectAccess mediante Djoin.exe.

### <a name="operating-system-requirements"></a>Requisitos de sistema operativo
Solo puede ejecutar Djoin.exe para DirectAccess en equipos que ejecutan Windows Server 2016, Windows Server 2012 o Windows 8. El equipo en el que se ejecuta Djoin.exe para aprovisionar datos de la cuenta de equipo en AD DS debe ejecutar Windows Server 2016, Windows 10, Windows Server 2012 o Windows 8. El equipo que desea unir al dominio también debe ejecutar Windows Server 2016, Windows 10, Windows Server 2012 o Windows 8.

### <a name="credential-requirements"></a>Requisitos de credenciales
Para realizar una Unión a un dominio sin conexión, debe tener los derechos necesarios para unir las estaciones de trabajo al dominio. Los miembros del grupo Admins. del dominio tienen estos derechos de manera predeterminada. Si no es miembro del grupo Admins. del dominio, un miembro del grupo Admins. del dominio debe completar una de las siguientes acciones para que pueda unir las estaciones de trabajo al dominio:

-   Use directiva de grupo para conceder los derechos de usuario necesarios. Este método permite crear equipos en el contenedor equipos predeterminado y en cualquier unidad organizativa (OU) que se cree posteriormente (si no se agregan entradas de control de acceso (ACE) de denegación).

-   Edite la lista de control de acceso (ACL) del contenedor equipos predeterminados del dominio para delegar los permisos correctos.

-   Cree una unidad organizativa y edite la ACL en esa unidad organizativa para concederle el permiso **crear secundario-permitir** . Pase el parámetro **/machineOU** al comando **djoin/provision** .

Los procedimientos siguientes muestran cómo conceder derechos de usuario con directiva de grupo y cómo delegar los permisos correctos.

#### <a name="granting-user-rights-to-join-workstations-to-the-domain"></a>Conceder derechos de usuario para unir estaciones de trabajo al dominio
Puede usar el Consola de administración de directivas de grupo (GPMC) para modificar la Directiva de dominio o crear una directiva nueva que tenga la configuración que concede al usuario derechos para agregar estaciones de trabajo a un dominio.

La pertenencia al grupo **Admins**. del dominio, o equivalente, es lo mínimo necesario para conceder derechos de usuario.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477) ( https://go.microsoft.com/fwlink/?LinkId=83477) .

###### <a name="to-grant-rights-to-join-workstations-to-a-domain"></a>Para conceder derechos para unir estaciones de trabajo a un dominio

1.  Haga clic en **Inicio**, en **Herramientas administrativas** y, a continuación, en **Administración de directivas de grupo**.

2.  Haga doble clic en el nombre del bosque, haga doble clic en **dominios**, haga doble clic en el nombre del dominio al que desea unirse un equipo, haga clic con el botón secundario en **directiva predeterminada de dominio** y, a continuación, haga clic en **Editar**.

3.  En el árbol de consola, haga doble clic en **configuración del equipo**, haga doble clic en **directivas**, haga doble clic en **configuración de Windows**, haga doble clic en **configuración de seguridad**, haga doble clic en **Directivas locales** y, a continuación, haga doble clic en **asignación de derechos de usuario**.

4.  En el panel de detalles, haga doble clic en **Agregar estaciones de trabajo al dominio**.

5.  Active la casilla **definir esta configuración de directiva** y, a continuación, haga clic en **Agregar usuario o grupo**.

6.  Escriba el nombre de la cuenta a la que desea conceder derechos de usuario y, a continuación, haga clic en **Aceptar** dos veces.

## <a name="offline-domain-join-process"></a><a name="BKMK_ODKSxS"></a>Proceso de unión a dominio sin conexión
Ejecute Djoin.exe en un símbolo del sistema con privilegios elevados para aprovisionar los metadatos de la cuenta de equipo. Al ejecutar el comando de aprovisionamiento, los metadatos de la cuenta de equipo se crean en un archivo binario que se especifica como parte del comando.

Para obtener más información sobre la función NetProvisionComputerAccount que se usa para aprovisionar la cuenta de equipo durante una Unión a un dominio sin conexión, vea [función NetProvisionComputerAccount](https://go.microsoft.com/fwlink/?LinkId=162426) ( https://go.microsoft.com/fwlink/?LinkId=162426) . Para obtener más información acerca de la función NetRequestOfflineDomainJoin que se ejecuta localmente en el equipo de destino, vea [función NetRequestOfflineDomainJoin](https://go.microsoft.com/fwlink/?LinkId=162427) ( https://go.microsoft.com/fwlink/?LinkId=162427) .

## <a name="steps-for-performing-a-directaccess-offline-domain-join"></a><a name="BKMK_ODJSteps"></a>Pasos para realizar una Unión a un dominio sin conexión de DirectAccess
El proceso de unión al dominio sin conexión incluye los pasos siguientes:

1.  Cree una nueva cuenta de equipo para cada uno de los clientes remotos y genere un paquete de aprovisionamiento mediante el comando Djoin.exe desde un equipo unido a un dominio ya existente en la red corporativa.

2.  Agregar el equipo cliente al grupo de seguridad Clientesdirectaccess

3.  Transfiera el paquete de aprovisionamiento de forma segura a los equipos remotos que se van a unir al dominio.

4.  Aplique el paquete de aprovisionamiento y una el cliente al dominio.

5.  Reinicie el cliente para completar la Unión al dominio y establecer la conectividad.

Hay dos opciones que se deben tener en cuenta al crear el paquete de aprovisionamiento para el cliente. Si utilizó el Asistente para Introducción para instalar DirectAccess sin PKI, debe usar la opción 1 a continuación. Si utilizó el Asistente para configuración avanzada para instalar DirectAccess con PKI, debe usar la opción 2 a continuación.

Complete los pasos siguientes para realizar la Unión a un dominio sin conexión:

##### <a name="option1-create-a-provisioning-package-for-the-client-without-pki"></a>Opción1: crear un paquete de aprovisionamiento para el cliente sin PKI

1.  En un símbolo del sistema del servidor de acceso remoto, escriba el siguiente comando para aprovisionar la cuenta de equipo:

    ```
    Djoin /provision /domain <your domain name> /machine <remote machine name> /policynames DA Client GPO name /rootcacerts /savefile c:\files\provision.txt /reuse
    ```

##### <a name="option2-create-a-provisioning-package-for-the-client-with-pki"></a>Opción2: crear un paquete de aprovisionamiento para el cliente con PKI

1.  En un símbolo del sistema del servidor de acceso remoto, escriba el siguiente comando para aprovisionar la cuenta de equipo:

    ```
    Djoin /provision /machine <remote machine name> /domain <Your Domain name> /policynames <DA Client GPO name> /certtemplate <Name of client computer cert template> /savefile c:\files\provision.txt /reuse
    ```

##### <a name="add-the-client-computer-to-the-directaccessclients-security-group"></a>Agregar el equipo cliente al grupo de seguridad Clientesdirectaccess

1.  En el controlador de dominio, en la pantalla **Inicio** , escriba **activo** y seleccione **Active Directory pantalla usuarios y equipos** de la **aplicación** .

2.  Expanda el árbol bajo su dominio y seleccione el contenedor **usuarios** .

3.  En el panel de detalles, haga clic con el botón secundario en **clientesdirectaccess** y haga clic en **propiedades**.

4.  En la pestaña **Miembros**, haga clic en **Agregar**.

5.  Haga clic en **Tipos de objeto**, seleccione **Equipos** y, a continuación, haga clic en **Aceptar**.

6.  Escriba el nombre de cliente que desea agregar y, a continuación, haga clic en **Aceptar**.

7.  Haga clic en **Aceptar** para cerrar el cuadro de diálogo Propiedades de **clientesdirectaccess** y, a continuación, cierre **Active Directory usuarios y equipos**.

##### <a name="copy-and-then-apply-the-provisioning-package-to-the-client-computer"></a>Copiar y, a continuación, aplicar el paquete de aprovisionamiento en el equipo cliente

1.  Copie el paquete de aprovisionamiento de c:\files\provision.txt en el servidor de acceso remoto, donde se guardó, para c:\provision\provision.txt en el equipo cliente.

2.  En el equipo cliente, abra un símbolo del sistema con privilegios elevados y, a continuación, escriba el siguiente comando para solicitar la Unión al dominio:

    ```
    Djoin /requestodj /loadfile C:\provision\provision.txt /windowspath %windir% /localos
    ```

3.  Reinicie el equipo cliente. El equipo se unirá al dominio. Después del reinicio, el cliente se unirá al dominio y tendrá conectividad con la red corporativa con DirectAccess.

## <a name="see-also"></a>Consulte también
[NetProvisionComputerAccount función](https://go.microsoft.com/fwlink/?LinkId=162426) 
 ) [NetRequestOfflineDomainJoin función](https://go.microsoft.com/fwlink/?LinkId=162427) )




---
title: Introducción a las cuentas de servicio administradas de grupo
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7130ad73-9688-4f64-aca1-46a9187a46cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 6da212185eb47d3f30f81ca6f1eb322bc232e229
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881456"
---
# <a name="getting-started-with-group-managed-service-accounts"></a>Introducción a las cuentas de servicio administradas de grupo

>Se aplica a: Windows Server (canal semianual), Windows Server 2016


Esta guía proporciona instrucciones paso a paso e información general para habilitar y usar cuentas de servicio administradas de grupo en Windows Server 2012.

**En este documento**

-   [Requisitos previos](#BKMK_Prereqs)

-   [Introducción](#BKMK_Intro)

-   [Implementar una nueva granja de servidores](#BKMK_DeployNewFarm)

-   [Adición de hosts miembros a una granja de servidores existente](#BKMK_AddMemberHosts)

-   [Actualizando las propiedades de la cuenta de servicio administrada de grupo](#BKMK_Update_gMSA)

-   [Retirada de hosts miembros de una granja de servidores existente](#BKMK_DecommMemberHosts)


> [!NOTE]
> Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para más información, consulta [Uso de cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).

## <a name="BKMK_Prereqs"></a>Requisitos previos
Consulte la sección [Requisitos de las cuentas de servicio administradas de grupo](#BKMK_gMSA_Req)de este tema.

## <a name="BKMK_Intro"></a>Introducción
Cuando un equipo cliente se conecta a un servicio hospedado en una granja de servidores con equilibrio de carga de red (NLB) o algún otro método en el que todos los servidores aparezcan como un mismo servicio de cara al cliente, no se pueden usar protocolos de autenticación que admitan la autenticación mutua, como Kerberos, salvo que todas las instancias de los servicios utilicen la misma entidad de seguridad. Esto implica que todos los servicios tienen que usar las mismas contraseñas o claves para demostrar su identidad.

> [!NOTE]
> Los clústeres de conmutación por error no admiten las cuentas de servicio administradas de grupo (gMSA). Sin embargo, los servicios que se ejecutan sobre el Servicio de clúster pueden utilizar una gMSA o una cuenta de servicio administrada independiente (sMSA) si son servicios de Windows, grupos de aplicaciones o tareas programadas, o si admiten gSMA o sMSA de forma nativa.

Los servicios tienen las siguientes entidades de seguridad entre las que pueden elegir, y cada una de ellas tiene determinadas limitaciones.

|Entidades de seguridad|Ámbito|Servicios admitidos|Administración de contraseñas|
|-------|-----|-----------|------------|
|Cuenta de equipo del sistema de Windows|Dominio|Limitado a un servidor unido a un dominio|El equipo administra|
|Cuenta de equipo sin sistema de Windows|Dominio|Cualquier servidor unido a un dominio|Ninguno|
|Cuenta virtual|Local|Limitado a un servidor|El equipo administra|
|Cuenta de servicio administrada independiente de Windows 7|Dominio|Limitado a un servidor unido a un dominio|El equipo administra|
|Cuenta de usuario|Dominio|Cualquier servidor unido a un dominio|Ninguno|
|Cuenta de servicio administrada de grupo|Dominio|Cualquier servidor unido a un dominio de Windows Server 2012|El controlador de dominio administra y el host recupera|

No se pueden compartir entre varios sistemas las cuentas de equipo de Windows, las cuentas de servicio administradas independientes (sMSA) de Windows 7 ni las cuentas virtuales. Si configuras una cuenta para que la compartan los servicios de las granjas de servidores, tendrás que elegir una cuenta de usuario o una cuenta de equipo aparte de un sistema de Windows. En cualquiera de estos dos casos, las cuentas no tienen la funcionalidad de administrar contraseñas con un solo punto de control. Esto genera un problema: cada organización se ve obligada a crear una solución costosa para actualizar las claves del servicio en Active Directory y, luego, distribuir las claves a todas las instancias de esos servicios.

Con Windows Server 2012, servicios o los administradores de servicios no es necesario administrar la sincronización de contraseñas entre instancias de servicio al usar cuentas de servicio administradas de grupo (gMSA). Se aprovisiona la gMSA en AD y, después, se configura el servicio que admite las cuentas de servicio administradas. Puedes aprovisionar una gMSA con los cmdlets *-ADServiceAccount que forman parte del módulo de Active Directory. Admiten la configuración de identidades de servicio en el host:

-   Las mismas API que sMSA, de modo que los productos que admiten sMSA admiten también gMSA.

-   Los servicios que utilizan el Administrador de control de servicios para configurar la identidad de inicio de sesión.

-   Los servicios que utilizan el Administrador de IIS para los grupos de aplicaciones con el fin de configurar la identidad.

-   Las tareas que utilizan el Programador de tareas.

### <a name="BKMK_gMSA_Req"></a>Requisitos para las cuentas de servicio administradas de grupo
En la siguiente tabla, se indican los requisitos del sistema operativo que se deben cumplir para que la autenticación Kerberos funcione con los servicios que usan gMSA. Los requisitos de Active Directory se indican debajo de la tabla.

Para ejecutar los comandos de Windows PowerShell que se usan para administrar cuentas de servicio administradas de grupo, se necesita una arquitectura de 64 bits.

**Requisitos del sistema operativo**

|Elemento|Requisitos|Sistema operativo|
|------|--------|----------|
|Host de la aplicación cliente|Cliente Kerberos que cumpla RFC|Windows XP como mínimo|
|Los controladores de dominio del dominio de la cuenta de usuario|KDC que cumpla RFC|Windows Server 2003 como mínimo|
|Hosts miembros de los servicios compartidos|| Windows Server 2012 |
|Dominio del host miembro controladores de dominio|KDC que cumpla RFC|Windows Server 2003 como mínimo|
|los controladores de dominio del dominio de la cuenta de gMSA| Controladores de dominio de Windows Server 2012 disponibles para el host recupere la contraseña|Dominio con Windows Server 2012, que puede tener algunos sistemas anteriores a Windows Server 2012 |
|Host del servicio backend|Servidor de aplicaciones Kerberos que cumpla RFC|Windows Server 2003 como mínimo|
|Los controladores de dominio del dominio de la cuenta del servicio de back-end|KDC que cumpla RFC|Windows Server 2003 como mínimo|
|Windows PowerShell para Active Directory|Windows PowerShell para Active Directory instalado localmente en un equipo que admita una arquitectura de 64 bits o en el equipo de administración remoto (por ejemplo, con el kit de herramientas de administración remota del servidor)| Windows Server 2012 |

**Requisitos de servicio de dominio de Active Directory**

-   El esquema de Active Directory en el bosque del dominio de la gMSA debe actualizarse a Windows Server 2012 para crear una gMSA.

    Puede actualizar el esquema instalando un controlador de dominio que ejecuta Windows Server 2012 o mediante la ejecución de la versión de adprep.exe desde un equipo que ejecuta Windows Server 2012. El valor del atributo object-version del objeto CN=Schema,CN=Configuration,DC=Contoso,DC=Com debe ser 52.

-   Nueva cuenta gMSA aprovisionada

-   Si administras el permiso del host de servicios para usar gMSA por grupo, un grupo de seguridad nuevo o existente

-   Si administras el control de acceso a los servicios por grupo, un grupo de seguridad nuevo o existente

-   Si no está implementada en el dominio o no se creó la primera clave raíz maestra de Active Directory, créala. Se puede comprobar el resultado de la creación en el registro operativo KdsSvc, identificador de evento 4004.

Para obtener instrucciones sobre cómo crear la clave, consulte [crear la clave raíz de KDS de servicios de clave de distribución](create-the-key-distribution-services-kds-root-key.md). Servicio de distribución de claves de Microsoft (kdssvc.dll): la clave raíz de AD.

**Ciclo de vida**

El ciclo de vida de una granja de servidores que utiliza la característica de gMSA suele incluir las siguientes tareas:

-   Implementación de una nueva granja de servidores

-   Adición de hosts miembros a una granja de servidores existente

-   Retirada de hosts miembros de una granja de servidores existente

-   Retirada de una granja de servidores existente

-   Eliminación de un host miembro que perdió su carácter confidencial de una granja de servidores si es necesario.

## <a name="BKMK_DeployNewFarm"></a>Implementar una nueva granja de servidores
Al implementar una nueva granja de servidores, el administrador de servicios tendrá que averiguar:

-   Si el servicio admite el uso de gMSA.

-   Si el servicio requiere conexiones autenticadas de entrada o de salida.

-   Los nombres de las cuentas de equipo de los hosts miembros del servicio que usan la gMSA.

-   El nombre NetBIOS del servicio.

-   El nombre del host DNS del servicio.

-   Los nombres de las entidades de servicio (SPN) del servicio.

-   El intervalo de cambio de la contraseña (el valor predeterminado es 30 días).

### <a name="BKMK_Step1"></a>Paso 1: Aprovisionamiento de cuentas de servicio administradas de grupo
Puede crear una gMSA solo si se ha actualizado el esquema del bosque a Windows Server 2012, se ha implementado la clave raíz maestra de Active Directory y hay al menos un DC de Windows Server 2012 en el dominio en el que se creará la gMSA.

Para completar los siguientes procedimientos, el requisito mínimo es ser miembro de **Admins. del dominio**u **Opers. de cuentas** o poder crear objetos msDS-GroupManagedServiceAccount.

#### <a name="BKMK_CreateGMSA"></a>Para crear una gMSA con el cmdlet New-ADServiceAccount

1.  En el controlador de dominio de Windows Server 2012, ejecute Windows PowerShell desde la barra de tareas.

2.  Escribe los siguientes comandos en el símbolo del sistema de Windows PowerShell y, luego, presiona ENTRAR. (El módulo de Active Directory se cargará automáticamente).

    **Nuevo-ADServiceAccount [-nombre] <string> - DNSHostName <string> [-KerberosEncryptionType <ADKerberosEncryptionType>] [-ManagedPasswordIntervalInDays < que aceptan valores null [Int32] >] [-PrincipalsAllowedToRetrieveManagedPassword < [] ADPrincipal >] - SamAccountName <string> - ServicePrincipalNames < string [] >**

    |Parámetro|Cadena|Ejemplo|
    |-------|-----|------|
    |Nombre|Nombre de la cuenta|ITFarm1|
    |DNSHostName|Nombre del host DNS del servicio|ITFarm1.contoso.com|
    |KerberosEncryptionType|Todos los tipos de cifrado admitidos por los servidores host|RC4, AES128, AES256|
    |ManagedPasswordIntervalInDays|Intervalo de cambio de la contraseña en días (si no se especifica, el valor predeterminado es 30 días)|90|
    |PrincipalsAllowedToRetrieveManagedPassword|Las cuentas de equipo de los hosts miembros o el grupo de seguridad al que pertenecen los hosts miembros|ITFarmHosts|
    |SamAccountName|Nombre NetBIOS del servicio, si no es el mismo que el de Name|ITFarm1|
    |ServicePrincipalNames|Nombres de las entidades de servicio (SPN) del servicio|http/ITFarm1.contoso.com/contoso.com, http/ITFarm1.contoso.com/contoso, http/ITFarm1/contoso.com, http/ITFarm1/contoso|

    > [!IMPORTANT]
    > El intervalo de cambio de la contraseña solo se puede definir durante la creación. Si necesitas cambiar este intervalo, tendrás que crear una nueva gMSA y definir el intervalo al crearla.

    **Ejemplo**

    Escribe el comando en una sola línea, aunque aquí pueda aparecer con saltos de línea debido a las limitaciones del formato.

    ```
    New-ADServiceAccount ITFarm1 -DNSHostName ITFarm1.contoso.com -PrincipalsAllowedToRetrieveManagedPassword ITFarmHosts -KerberosEncryptionType RC4, AES128, AES256 -ServicePrincipalNames http/ITFarm1.contoso.com/contoso.com, http/ITFarm1.contoso.com/contoso, http/ITFarm1/contoso.com, http/ITFarm1/contoso

    ```

Para completar este procedimiento, el requisito mínimo es ser miembro de **Admins. del dominio** u **Opers. de cuentas** o poder crear objetos msDS-GroupManagedServiceAccount. Para ver información detallada sobre el uso de las cuentas adecuadas y las pertenencias a grupos, consulte [Grupos predeterminados locales y de dominio](https://technet.microsoft.com/library/dd728026(WS.10).aspx).

##### <a name="to-create-a-gmsa-for-outbound-authentication-only-using-the-new-adserviceaccount-cmdlet"></a>Para crear una gMSA de autenticación de salida solamente con el cmdlet New-ADServiceAccount

1.  En el controlador de dominio de Windows Server 2012, ejecute Windows PowerShell desde la barra de tareas.

2.  Escribe los siguientes comandos en el símbolo del sistema del módulo de Active Directory de Windows PowerShell y, luego, presiona ENTRAR:

    **New-ADServiceAccount [-Name] <string> -RestrictToOutboundAuthenticationOnly [-ManagedPasswordIntervalInDays <Nullable[Int32]>] [-PrincipalsAllowedToRetrieveManagedPassword <ADPrincipal[]>]**

    |Parámetro|Cadena|Ejemplo|
    |-------|-----|------|
    |Nombre|Nombre de la cuenta|ITFarm1|
    |ManagedPasswordIntervalInDays|Intervalo de cambio de la contraseña en días (si no se especifica, el valor predeterminado es 30 días)|75|
    |PrincipalsAllowedToRetrieveManagedPassword|Las cuentas de equipo de los hosts miembros o el grupo de seguridad al que pertenecen los hosts miembros|ITFarmHosts|

    > [!IMPORTANT]
    > El intervalo de cambio de la contraseña solo se puede definir durante la creación. Si necesitas cambiar este intervalo, tendrás que crear una nueva gMSA y definir el intervalo al crearla.

**Ejemplo**

```
New-ADServiceAccount ITFarm1 -RestrictToOutboundAuthenticationOnly - PrincipalsAllowedToRetrieveManagedPassword ITFarmHosts

```

### <a name="BKMK_ConfigureServiceIdentity"></a>Paso 2: Configuración del servicio de aplicación de identidad de servicio
Para configurar los servicios en Windows Server 2012, consulte la documentación de la característica siguiente:

-   Grupo de aplicaciones de IIS

    Para obtener más información, consulte [Especificar una identidad para un grupo de aplicaciones (IIS 7)](https://technet.microsoft.com/library/cc771170(WS.10).aspx).

-   Servicios de Windows

    Para obtener más información, consulte [Servicios](https://technet.microsoft.com/library/cc772408.aspx).

-   Tareas

    Para obtener más información, consulte la [Introducción al Programador de tareas](https://technet.microsoft.com/library/cc721871.aspx).

Puede haber otros servicios que admitan gMSA. Para ver información detallada sobre cómo configurar esos servicios, consulta la documentación del producto correspondiente.

## <a name="BKMK_AddMemberHosts"></a>Adición de hosts miembros a una granja de servidores existente
Si usa grupos de seguridad para administrar los hosts miembros, agregue la cuenta de equipo para el nuevo host miembro al grupo de seguridad (que hosts miembros de la gMSA están un miembro de) mediante uno de los métodos siguientes.

Para completar estos procedimientos, el requisito mínimo es ser miembro de **Admins. del dominio**o poder agregar miembros al objeto del grupo de seguridad.

-   Método 1: Usuarios y equipos de Active Directory

    Para ver los procedimientos de este método, consulte [Agregarcuentas de equipo a grupos](https://technet.microsoft.com/library/cc733097.aspx) con la interfaz de Windows y [Administrar dominios diferentes en el Centro de administración de Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

-   Método 2: dsmod

    Para ver los procedimientos de este método, consulte [Agregar cuentas de equipo a grupos](https://technet.microsoft.com/library/cc733097.aspx) con la línea de comandos.

-   Método 3: cmdlet de Active Directory de Windows PowerShell Add-ADPrincipalGroupMembership

    Para ver los procedimientos de este método, consulte [Add-ADPrincipalGroupMembership](https://technet.microsoft.com/library/ee617203.aspx).

Si usas cuentas de equipo, busca las cuentas existentes y agrega la nueva cuenta de equipo.

Para completar este procedimiento, el requisito mínimo es ser miembro de **Admins. del dominio**u **Opers. de cuentas**o poder administrar objetos msDS-GroupManagedServiceAccount. Para ver información detallada sobre el uso de las cuentas adecuadas y las pertenencias a grupos, consulte Grupos predeterminados locales y de dominio.

#### <a name="to-add-member-hosts-using-the-set-adserviceaccount-cmdlet"></a>Para agregar hosts miembros con el cmdlet Set-ADServiceAccount

1.  En el controlador de dominio de Windows Server 2012, ejecute Windows PowerShell desde la barra de tareas.

2.  Escribe los siguientes comandos en el símbolo del sistema del módulo de Active Directory de Windows PowerShell y, luego, presiona ENTRAR:

    **Get-ADServiceAccount [-Name] <string> -PrincipalsAllowedToRetrieveManagedPassword**

3.  Escribe los siguientes comandos en el símbolo del sistema del módulo de Active Directory de Windows PowerShell y, luego, presiona ENTRAR:

    **Set-ADServiceAccount [-Name] <string> -PrincipalsAllowedToRetrieveManagedPassword <ADPrincipal[]>**

|Parámetro|Cadena|Ejemplo|
|-------|-----|------|
|Name|Nombre de la cuenta|ITFarm1|
|PrincipalsAllowedToRetrieveManagedPassword|Las cuentas de equipo de los hosts miembros o el grupo de seguridad al que pertenecen los hosts miembros|Host1, Host2, Host3|

**Ejemplo**

Por ejemplo, para agregar hosts miembros, escribe los siguientes comandos y, luego, presiona ENTRAR.

```
Get-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword

```

```
Set-ADServiceAccount [-Name] ITFarm1-PrincipalsAllowedToRetrieveManagedPassword Host1,Host2,Host3

```

## <a name="BKMK_Update_gMSA"></a>Actualizando las propiedades de la cuenta de servicio administrada de grupo
Para completar estos procedimientos, el requisito mínimo es ser miembro de **Admins. del dominio**u **Opers. de cuentas**o poder escribir en objetos msDS-GroupManagedServiceAccount.

Abre el módulo de Active Directory para Windows PowerShell y define las propiedades con el cmdlet Set-ADServiceAccount.

Para ver información detallada sobre cómo definir estas propiedades, consulte [Set-ADServiceAccount](https://technet.microsoft.com/library/ee617252.aspx) en la biblioteca de TechNet o escriba **Get-Help Set-ADServiceAccount** en el símbolo del sistema del módulo de Active Directory para Windows PowerShell y presione ENTRAR para consultarlo.

## <a name="BKMK_DecommMemberHosts"></a>Retirada de hosts miembros de una granja de servidores existente
Para completar estos procedimientos, el requisito mínimo es ser miembro de **Admins. del dominio** o poder quitar miembros del objeto del grupo de seguridad.

### <a name="step-1-remove-member-host-from-gmsa"></a>Paso 1: Eliminación del host miembro de la gMSA
Si usa grupos de seguridad para administrar los hosts miembros, quite la cuenta de equipo para el host miembro retirado del grupo de seguridad que los hosts miembros de la gMSA están miembro de mediante cualquiera de los métodos siguientes.

-   Método 1: Usuarios y equipos de Active Directory

    Para ver los procedimientos de este método, consulte [Eliminar cuentas de equipo](https://technet.microsoft.com/library/cc754624.aspx) con la interfaz de Windows y [Administrar dominios diferentes en el Centro de administración de Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

-   Método 2: drsm

    Para ver los procedimientos de este método, consulte [Eliminar cuentas de equipo](https://technet.microsoft.com/library/cc754624.aspx) con la línea de comandos.

-   Método 3: cmdlet de Active Directory de Windows PowerShell Remove-ADPrincipalGroupMembership

    Para ver información detallada sobre cómo hacer esto, consulte  [Remove-ADPrincipalGroupMembership](https://technet.microsoft.com/library/ee617243.aspx) en la biblioteca de TechNet o escriba **Get-Help Remove-ADPrincipalGroupMembership** en el símbolo del sistema del módulo de Active Directory para Windows PowerShell y presione ENTRAR para consultarlo.

Si usas listas de cuentas de equipo, recupera las cuentas existentes y, luego, agrega todas menos la cuenta del equipo quitado.

Para completar este procedimiento, el requisito mínimo es ser miembro de **Admins. del dominio**u **Opers. de cuentas**o poder administrar objetos msDS-GroupManagedServiceAccount. Para ver información detallada sobre el uso de las cuentas adecuadas y las pertenencias a grupos, consulte Grupos predeterminados locales y de dominio.

##### <a name="to-remove-member-hosts-using-the-set-adserviceaccount-cmdlet"></a>Para quitar hosts miembros con el cmdlet Set-ADServiceAccount

1.  En el controlador de dominio de Windows Server 2012, ejecute Windows PowerShell desde la barra de tareas.

2.  Escribe los siguientes comandos en el símbolo del sistema del módulo de Active Directory de Windows PowerShell y, luego, presiona ENTRAR:

    **Get-ADServiceAccount [-Name] <string> -PrincipalsAllowedToRetrieveManagedPassword**

3.  Escribe los siguientes comandos en el símbolo del sistema del módulo de Active Directory de Windows PowerShell y, luego, presiona ENTRAR:

    **Set-ADServiceAccount [-Name] <string> -PrincipalsAllowedToRetrieveManagedPassword <ADPrincipal[]>**

|Parámetro|Cadena|Ejemplo|
|-------|-----|------|
|Nombre|Nombre de la cuenta|ITFarm1|
|PrincipalsAllowedToRetrieveManagedPassword|Las cuentas de equipo de los hosts miembros o el grupo de seguridad al que pertenecen los hosts miembros|Host1, Host3|

**Ejemplo**

Por ejemplo, para quitar hosts miembros, escribe los siguientes comandos y, luego, presiona ENTRAR.

```
Get-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword

```

```
Set-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword Host1,Host3

```

### <a name="BKMK_RemoveGMSA"></a>Paso 2: Eliminación de una cuenta de servicio administrada de grupo del sistema
Quita del host miembro las credenciales almacenadas en caché de la gMSA con Uninstall-ADServiceAccount o la API NetRemoveServiceAccount en el sistema host.

El requisito mínimo para completar estos procedimientos es pertenecer al grupo **Administradores** u otro equivalente.

##### <a name="to-remove-a-gmsa-using-the-uninstall-adserviceaccount-cmdlet"></a>Para quitar una gMSA con el cmdlet Uninstall-ADServiceAccount

1.  En el controlador de dominio de Windows Server 2012, ejecute Windows PowerShell desde la barra de tareas.

2.  Escribe los siguientes comandos en el símbolo del sistema del módulo de Active Directory de Windows PowerShell y, luego, presiona ENTRAR:

    **Uninstall-ADServiceAccount < ADServiceAccount>**

    **Ejemplo**

    Por ejemplo, para quitar las credenciales almacenadas en caché de una gMSA llamada ITFarm1, escribe el siguiente comando y presiona ENTRAR:

    ```
    Uninstall-ADServiceAccount ITFarm1
    ```

Para obtener más información sobre el cmdlet Uninstall-ADServiceAccount, en el símbolo del sistema del módulo de Active Directory para Windows PowerShell, escriba **Get-Help Uninstall-ADServiceAccount**y presione ENTRAR o consulte la información de la web de TechNet en [Uninstall-ADServiceAccount](https://technet.microsoft.com/library/ee617202.aspx).



## <a name="BKMK_Links"></a>Vea también

-   [Introducción a las cuentas de servicio administradas de grupo](group-managed-service-accounts-overview.md)




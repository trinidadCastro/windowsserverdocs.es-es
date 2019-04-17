---
title: "Introducción a grupo administra las cuentas de servicio"
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
ms.openlocfilehash: cd1e92f93701e5b430d1425fcf9a2f3590d1fe5d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="getting-started-with-group-managed-service-accounts"></a>Introducción a grupo administra las cuentas de servicio

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016


Esta guía proporciona instrucciones paso a paso e información general para habilitar y usar cuentas de servicio administradas de grupo en Windows Server 2012.

**En este documento**

-   [Requisitos previos](#BKMK_Prereqs)

-   [Introducción](#BKMK_Intro)

-   [Implementar un nuevo conjunto de servidor](#BKMK_DeployNewFarm)

-   [Adición de hosts de miembro a un conjunto de servidor existente](#BKMK_AddMemberHosts)

-   [Actualización de las propiedades de la cuenta de servicio administrada de grupo](#BKMK_Update_gMSA)

-   [La retirada de hosts de miembros de una granja de servidores existente](#BKMK_DecommMemberHosts)


> [!NOTE]
> Este tema incluye cmdlets de Windows PowerShell de muestra que puedes usar para automatizar algunos de los procedimientos descritos. Para obtener más información, consulta [uso de Cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).

## <a name="BKMK_Prereqs"></a>Requisitos previos
Consulta la sección de este tema en [requisitos para cuentas de servicio administradas grupo](#BKMK_gMSA_Req).

## <a name="BKMK_Intro"></a>Introducción
Cuando un equipo cliente se conecta a un servicio que se hospeda en una granja de servidores con (NLB) de equilibrio de carga de red o algún otro método donde todos los servidores parecen ser el mismo servicio al cliente, a continuación, compatibilidad con la autenticación mutua como Kerberos los protocolos de autenticación no se pueden usar a menos que todas las instancias de los servicios de usan a la misma identidad. Esto significa que cada servicio tiene que usar las mismas contraseñas y claves para probar su identidad.

> [!NOTE]
> Clústeres de conmutación por error no se admiten gMSAs. Sin embargo, los servicios que se ejecutan en el servicio de clúster usar un gMSA o un sMSA si son un servicio de Windows, un grupo de la aplicación, una tarea programada o admiten de forma nativa gMSA o sMSA.

Los servicios tienen los siguientes principales para elegir, y cada uno tiene algunas limitaciones.

|Entidades de seguridad|Ámbito|Servicios admitidos|Administración de contraseñas|
|-------|-----|-----------|------------|
|Sistema de la cuenta de Windows|Dominio|Limitado a un dominio unido a un servidor|Administra el equipo|
|Cuenta de equipo sin sistema de Windows|Dominio|Los servidores de estén unidos a un dominio|Ninguno|
|Cuenta virtual|Local|Limitado a un servidor|Administra el equipo|
|Cuenta de servicio administrado de independientes de Windows 7|Dominio|Limitado a un dominio unido a un servidor|Administra el equipo|
|Cuenta de usuario|Dominio|Los servidores de estén unidos a un dominio|Ninguno|
|Cuenta de grupo de servicio administrada|Dominio|Los servidores unidos a un dominio de Windows Server 2012|Administra el controlador de dominio y se recupera el host|

Una cuenta de equipo de Windows o una cuenta de servicio administrada (sMSA), independiente de Windows 7 o cuentas virtuales no se pueden compartir a través de varios sistemas. Si configuras una cuenta de servicios en conjuntos de servidores para compartir, tendrías que elegir una cuenta de usuario o una cuenta de equipo aparte de un sistema de Windows. De cualquier manera, estas cuentas no tienen la capacidad de administración de contraseñas solo punto de control. Esto crea problema donde cada organización necesita para crear una solución cara para actualizar las claves para el servicio en Active Directory y, a continuación, distribuir las claves para todas las instancias de esos servicios.

Con Windows Server 2012, los administradores de servicio o servicios no es necesario administrar la sincronización de contraseñas entre instancias de servicio al usar cuentas de servicio administradas de grupo (gMSA). Puedes aprovisionen la gMSA en AD y, a continuación, configura el servicio que admite cuentas de servicio administradas. Puedes aprovisionar una gMSA usando el *-cmdlets ADServiceAccount que forman parte del módulo de Active Directory. Configuración del servicio de identidad en el host es compatible con:

-   Mismas API que sMSA para que productos que admiten sMSA admitirá gMSA

-   Servicios que usan el Administrador de Control de servicio para configurar la identidad de inicio de sesión

-   Servicios que usan el Administrador de IIS para grupos de aplicaciones para configurar la identidad

-   Tareas con el programador de tareas.

### <a name="BKMK_gMSA_Req"></a>Requisitos para cuentas de servicio administradas de grupo
La siguiente tabla enumera los requisitos de sistema operativo para la autenticación Kerberos trabajar con servicios mediante gMSA. Después de la tabla, se enumeran los requisitos de Active Directory.

Una arquitectura de 64 bits se necesita para ejecutar los comandos de Windows PowerShell usados para administrar cuentas de servicio administradas de grupo.

**Requisitos del sistema operativo**

|Elemento|Requisito|Sistema operativo|
|------|--------|----------|
|Host de la aplicación de cliente|Cliente de Kerberos compatible RFC|Al menos Windows XP|
|Controladores de dominio del dominio de la cuenta de usuario|RFC compatible con KDC|Al menos Windows Server 2003|
|Hosts de miembros de servicio compartido|| Windows Server 2012 |
|Dominio del host de miembro controladores de dominio|RFC compatible con KDC|Al menos Windows Server 2003|
|dominio de la cuenta gMSA controladores de dominio| Controladores de dominio de Windows Server 2012 disponible para su host recuperar la contraseña|Dominio con Windows Server 2012, que puede tener algunos sistemas anteriores a Windows Server 2012 |
|Host del servicio back-end|Servidor de aplicaciones de RFC compatible con Kerberos|Al menos Windows Server 2003|
|Dominio de la cuenta de servicio back-end controladores de dominio|RFC compatible con KDC|Al menos Windows Server 2003|
|Windows PowerShell para Active Directory|Windows PowerShell para Active Directory que se instalan localmente en un equipo compatible con una arquitectura de 64 bits o en el equipo de administración remota (por ejemplo, mediante el Kit de herramientas de administración de servidor remoto)| Windows Server 2012 |

**Requisitos de servicio de directorio de dominio de Active**

-   El esquema de Active Directory en el bosque del dominio gMSA debe actualizarse a Windows Server 2012 para crear un gMSA.

    Puedes actualizar el esquema instalando un controlador de dominio que ejecute Windows Server 2012 o ejecutando la versión de adprep.exe desde un equipo que ejecute Windows Server 2012. El valor de atributo de versión del objeto para el objeto CN = esquema, CN = Configuración, DC = Contoso, DC = Com debe ser 52.

-   Nueva cuenta de gMSA aprovisionado

-   Si va a administrar el permiso de host de servicio para usar gMSA por grupo y, luego, en el grupo de seguridad nuevas o existentes

-   Si la administración del control de acceso del servicio por grupo y, luego, en el grupo de seguridad nuevas o existentes

-   Si la primera clave raíz maestro Active Directory no se ha implementado en el dominio o no se ha creado, a continuación, crear. En el registro operativo de KdsSvc, 4004 de identificador de evento, se puede comprobar el resultado de su creación.

Para obtener instrucciones cómo crear la clave, consulte [crear la clave de raíz de distribución servicios KDS](create-the-key-distribution-services-kds-root-key.md). Clave de distribución de servicio de Microsoft (kdssvc.dll) la clave raíz para anuncios.

**Ciclo de vida**

El ciclo de vida de una granja de servidores con la característica gMSA normalmente incluye las siguientes tareas:

-   Implementar un nuevo conjunto de servidor

-   Adición de hosts de miembro a un conjunto de servidor existente

-   La retirada de hosts de miembros de una granja de servidores existente

-   La retirada de una granja de servidores existente

-   Quitar un host de miembro en peligro una granja de servidores si es necesario.

## <a name="BKMK_DeployNewFarm"></a>Implementar un nuevo conjunto de servidor
Al implementar un nuevo conjunto de servidores, el Administrador de servicio, deberás determinar:

-   Si el servicio admite el uso de gMSAs

-   Si el servicio requiere conexiones autenticadas entrantes o salientes

-   Los nombres de cuenta de equipo para los hosts de miembro para el servicio con el gMSA

-   El nombre NetBIOS del servicio

-   El nombre de host DNS para el servicio

-   Los nombres de entidad de seguridad de servicio (SPN) para el servicio

-   Cambiar la contraseña intervalo (el valor predeterminado es 30 días).

### <a name="BKMK_Step1"></a>Paso 1: Aprovisionamiento de cuentas de servicio administradas de grupo
Puedes crear un gMSA solo si el esquema del bosque se ha actualizado a Windows Server 2012, se haya implementado la clave maestra de raíz para Active Directory y no hay al menos un DC de Windows Server 2012 en el dominio en el que se creará el gMSA.

Pertenencia a **administradores de dominio**, **operadores de cuentas** o capacidad para crear objetos msDS GroupManagedServiceAccount, es lo mínimo necesario para completar los procedimientos siguientes.

#### <a name="BKMK_CreateGMSA"></a>Para crear un gMSA mediante el cmdlet New-ADServiceAccount

1.  En el controlador de dominio de Windows Server 2012, ejecutar Windows PowerShell desde la barra de tareas.

2.  En el símbolo del sistema de Windows PowerShell, escribe los siguientes comandos y, a continuación, presione ENTRAR. (El módulo de Active Directory se cargará automáticamente.)

    **Nuevo ADServiceAccount [-nombre] <string> - DNSHostName <string> [-KerberosEncryptionType <ADKerberosEncryptionType>] [-ManagedPasswordIntervalInDays < Nullable [Int32] >] [-PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [] >] SamAccountName - <string> - ServicePrincipalNames < string [] >**

    |Parámetro|Cadena|Ejemplo|
    |-------|-----|------|
    |Nombre|Nombre de la cuenta|ITFarm1|
    |DNSHostName|Nombre de host DNS de servicio|ITFarm1.contoso.com|
    |KerberosEncryptionType|Todos los tipos de cifrado compatibles con los servidores host|CIFRADO DE RC4, AES128, AES256|
    |ManagedPasswordIntervalInDays|Intervalo de cambio de contraseña en días (valor predeterminado es 30 días si no proporciona)|90|
    |PrincipalsAllowedToRetrieveManagedPassword|Las cuentas de equipo de los hosts de miembro o el grupo de seguridad que los hosts de miembro son miembros de|ITFarmHosts|
    |SamAccountName|Si no es así, nombre NetBIOS para el servicio es igual a nombre|ITFarm1|
    |ServicePrincipalNames|Nombres de entidad de seguridad de servicio (SPN) para el servicio|http/ITFarm1.contoso.com/contoso.com, http/ITFarm1.contoso.com/contoso, http/ITFarm1/contoso.com, http, ITFarm1/contoso|

    > [!IMPORTANT]
    > El intervalo de cambio de contraseña solo puede establecerse durante la creación. Si es necesario cambiar el intervalo de conexión, debes crear un nuevo gMSA y establecerlo en tiempo de creación.

    **Ejemplo**

    Escribe el comando en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.

    ```
    New-ADServiceAccount ITFarm1 -DNSHostName ITFarm1.contoso.com -PrincipalsAllowedToRetrieveManagedPassword ITFarmHosts -KerberosEncryptionType RC4, AES128, AES256 -ServicePrincipalNames http/ITFarm1.contoso.com/contoso.com, http/ITFarm1.contoso.com/contoso, http/ITFarm1/contoso.com, http/ITFarm1/contoso

    ```

Pertenencia a **administradores de dominio**, **operadores de cuentas**, o la capacidad para crear objetos msDS GroupManagedServiceAccount, es lo mínimo necesario para completar este procedimiento. Para obtener más información sobre el uso de las cuentas y adecuadas pertenencias a grupos, [Local y dominio predeterminada grupos](https://technet.microsoft.com/library/dd728026(WS.10).aspx).

##### <a name="to-create-a-gmsa-for-outbound-authentication-only-using-the-new-adserviceaccount-cmdlet"></a>Para crear un gMSA para la autenticación de salida solo mediante el cmdlet New-ADServiceAccount

1.  En el controlador de dominio de Windows Server 2012, ejecutar Windows PowerShell desde la barra de tareas.

2.  En el símbolo del sistema para el módulo de Active Directory de Windows PowerShell, escribe los siguientes comandos y, a continuación, presiona ENTRAR:

    **Nuevo ADServiceAccount [-nombre] <string> - RestrictToOutboundAuthenticationOnly [-ManagedPasswordIntervalInDays < que acepta valores null [Int32] >] [-PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [] >]**

    |Parámetro|Cadena|Ejemplo|
    |-------|-----|------|
    |Nombre|Nombre de la cuenta|ITFarm1|
    |ManagedPasswordIntervalInDays|Intervalo de cambio de contraseña en días (valor predeterminado es 30 días si no proporciona)|75|
    |PrincipalsAllowedToRetrieveManagedPassword|Las cuentas de equipo de los hosts de miembro o el grupo de seguridad que los hosts de miembro son miembros de|ITFarmHosts|

    > [!IMPORTANT]
    > El intervalo de cambio de contraseña solo puede establecerse durante la creación. Si es necesario cambiar el intervalo de conexión, debes crear un nuevo gMSA y establecerlo en tiempo de creación.

**Ejemplo**

```
New-ADServiceAccount ITFarm1 -RestrictToOutboundAuthenticationOnly - PrincipalsAllowedToRetrieveManagedPassword ITFarmHosts

```

### <a name="BKMK_ConfigureServiceIdentity"></a>Paso 2: Configurar el servicio de servicio identidad de aplicación
Para configurar los servicios de Windows Server 2012, consulta la siguiente documentación de la característica:

-   Grupo de aplicaciones de IIS

    Para obtener más información, consulta [especificar una identidad para un grupo de aplicaciones (IIS 7)](https://technet.microsoft.com/library/cc771170(WS.10).aspx).

-   Servicios de Windows

    Para obtener más información, consulta [servicios](https://technet.microsoft.com/library/cc772408.aspx).

-   Tareas

    Para obtener más información, consulta el [introducción del programador de tareas](https://technet.microsoft.com/library/cc721871.aspx).

Otros servicios pueden admitir gMSA. Consulta la documentación de producto apropiada para obtener más información sobre cómo configurar esos servicios.

## <a name="BKMK_AddMemberHosts"></a>Adición de hosts de miembro a un conjunto de servidor existente
Si usando grupos de seguridad para la administración de hosts miembros, agregar la cuenta de equipo para el nuevo host de miembro para el grupo de seguridad (que hosts de miembros de gMSA son miembros de) usando uno de los siguientes métodos.

Pertenencia a **administradores de dominio**, o la posibilidad de agregar miembros para el objeto de grupo de seguridad es lo mínimo necesario para completar estos procedimientos.

-   Método 1: Los usuarios de Active Directory y equipos

    Para conocer los procedimientos para usar este método, consulta [agregar una cuenta de equipo a un grupo](https://technet.microsoft.com/library/cc733097.aspx) mediante la interfaz de Windows, y [administrar distintos dominios en el centro de administración de Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

-   Método 2: dsmod

    Para conocer los procedimientos para usar este método, consulta [agregar una cuenta de equipo a un grupo](https://technet.microsoft.com/library/cc733097.aspx) mediante la línea de comandos.

-   Método 3: Cmdlet de Windows PowerShell Active Directory agregar ADPrincipalGroupMembership

    Para conocer los procedimientos para usar este método, consulta [agregar ADPrincipalGroupMembership](https://technet.microsoft.com/library/ee617203.aspx).

Si el uso de cuentas de equipo, busca las cuentas existentes y, a continuación, agrega la nueva cuenta de equipo.

Pertenencia a **administradores de dominio**, **operadores de cuentas**, o la capacidad para administrar objetos msDS GroupManagedServiceAccount, es lo mínimo necesario para completar este procedimiento. Para obtener información detallada sobre el uso de las cuentas y adecuadas pertenencias a grupos, consulta Local y grupos de dominio predeterminados.

#### <a name="to-add-member-hosts-using-the-set-adserviceaccount-cmdlet"></a>Para agregar los hosts de miembros mediante el cmdlet Set-ADServiceAccount

1.  En el controlador de dominio de Windows Server 2012, ejecutar Windows PowerShell desde la barra de tareas.

2.  En el símbolo del sistema para el módulo de Active Directory de Windows PowerShell, escribe los siguientes comandos y, a continuación, presiona ENTRAR:

    **Get-ADServiceAccount [-nombre] <string> - PrincipalsAllowedToRetrieveManagedPassword**

3.  En el símbolo del sistema para el módulo de Active Directory de Windows PowerShell, escribe los siguientes comandos y, a continuación, presiona ENTRAR:

    **Conjunto ADServiceAccount [-nombre] <string> - PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [] >**

|Parámetro|Cadena|Ejemplo|
|-------|-----|------|
|Nombre|Nombre de la cuenta|ITFarm1|
|PrincipalsAllowedToRetrieveManagedPassword|Las cuentas de equipo de los hosts de miembro o el grupo de seguridad que los hosts de miembro son miembros de|Host1, Host2, Host3|

**Ejemplo**

Por ejemplo, agregar miembro hosts escribe los siguientes comandos y, a continuación, presione ENTRAR.

```
Get-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword

```

```
Set-ADServiceAccount [-Name] ITFarm1-PrincipalsAllowedToRetrieveManagedPassword Host1 Host2 Host3

```

## <a name="BKMK_Update_gMSA"></a>Actualización de las propiedades de la cuenta de servicio administrada de grupo
Pertenencia a **administradores de dominio**, **operadores de cuentas**, o la capacidad de escribir en objetos msDS GroupManagedServiceAccount, es lo mínimo necesario para completar estos procedimientos.

Abre el Active Directory módulo de Windows PowerShell y establecer cualquier propiedad mediante el cmdlet Set-ADServiceAccount.

Para obtener información detallada acerca de cómo establecer estas propiedades, consulta [conjunto ADServiceAccount](https://technet.microsoft.com/library/ee617252.aspx) en la biblioteca de TechNet o escribiendo **Get-Help Set-ADServiceAccount** en el módulo de Active Directory para Windows PowerShell comando símbolo del sistema y al presionar ENTRAR.

## <a name="BKMK_DecommMemberHosts"></a>La retirada de hosts de miembros de una granja de servidores existente
Pertenencia a **administradores de dominio**, o la posibilidad de quitar miembros de objeto de grupo de seguridad, es lo mínimo necesario para completar estos procedimientos.

### <a name="step-1-remove-member-host-from-gmsa"></a>Paso 1: Quitar el host de miembro de gMSA
Si mediante grupos de seguridad para la administración de hosts miembros, quita la cuenta de equipo para el host de retiro miembro del grupo de seguridad que hosts de miembros de gMSA son miembros del uso de cualquiera de los siguientes métodos.

-   Método 1: Los usuarios de Active Directory y equipos

    Para conocer los procedimientos para usar este método, consulta [eliminar una cuenta de equipo](https://technet.microsoft.com/library/cc754624.aspx) mediante la interfaz de Windows, y [administrar distintos dominios en el centro de administración de Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

-   Método 2: drsm

    Para conocer los procedimientos para usar este método, consulta [eliminar una cuenta de equipo](https://technet.microsoft.com/library/cc754624.aspx) mediante la línea de comandos.

-   Método 3: Cmdlet de Windows PowerShell Active Directory quitar ADPrincipalGroupMembership

    Para obtener información detallada acerca de cómo hacerlo, consulta [quitar ADPrincipalGroupMembership](https://technet.microsoft.com/library/ee617243.aspx) en la biblioteca de TechNet o escribiendo **Get-Help Remove-ADPrincipalGroupMembership** en el módulo de Active Directory para Windows PowerShell comando símbolo del sistema y al presionar ENTRAR.

Si la lista de cuentas de equipo, recuperar las cuentas existentes y, a continuación, agrega todo excepto la cuenta de equipo eliminadas.

Pertenencia a **administradores de dominio**, **operadores de cuentas**, o la capacidad para administrar objetos msDS GroupManagedServiceAccount, es lo mínimo necesario para completar este procedimiento. Para obtener información detallada sobre el uso de las cuentas y adecuadas pertenencias a grupos, consulta Local y grupos de dominio predeterminados.

##### <a name="to-remove-member-hosts-using-the-set-adserviceaccount-cmdlet"></a>Para quitar los hosts de miembros mediante el cmdlet Set-ADServiceAccount

1.  En el controlador de dominio de Windows Server 2012, ejecutar Windows PowerShell desde la barra de tareas.

2.  En el símbolo del sistema para el módulo de Active Directory de Windows PowerShell, escribe los siguientes comandos y, a continuación, presiona ENTRAR:

    **Get-ADServiceAccount [-nombre] <string> - PrincipalsAllowedToRetrieveManagedPassword**

3.  En el símbolo del sistema para el módulo de Active Directory de Windows PowerShell, escribe los siguientes comandos y, a continuación, presiona ENTRAR:

    **Conjunto ADServiceAccount [-nombre] <string> - PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [] >**

|Parámetro|Cadena|Ejemplo|
|-------|-----|------|
|Nombre|Nombre de la cuenta|ITFarm1|
|PrincipalsAllowedToRetrieveManagedPassword|Las cuentas de equipo de los hosts de miembro o el grupo de seguridad que los hosts de miembro son miembros de|Host1, Host3|

**Ejemplo**

Por ejemplo, para quitar a miembros hosts escribe los siguientes comandos y, a continuación, presione ENTRAR.

```
Get-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword

```

```
Set-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword Host1 Host3

```

### <a name="BKMK_RemoveGMSA"></a>Paso 2: Quitar un cuenta de servicio administrado de grupo del sistema
Quitar las credenciales almacenadas en caché gMSA desde el host de miembro con desinstalar ADServiceAccount o la API de NetRemoveServiceAccount en el sistema host.

Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar estos procedimientos.

##### <a name="to-remove-a-gmsa-using-the-uninstall-adserviceaccount-cmdlet"></a>Para quitar un gMSA mediante el cmdlet desinstalar ADServiceAccount

1.  En el controlador de dominio de Windows Server 2012, ejecutar Windows PowerShell desde la barra de tareas.

2.  En el símbolo del sistema para el módulo de Active Directory de Windows PowerShell, escribe los siguientes comandos y, a continuación, presiona ENTRAR:

    **Desinstalar-ADServiceAccount < ADServiceAccount >**

    **Ejemplo**

    Por ejemplo, para quitar las credenciales almacenadas en caché para un gMSA ITFarm1 con nombre escribe el siguiente comando y, a continuación, presiona ENTRAR:

    ```
    Uninstall-ADServiceAccount ITFarm1
    ```

Para obtener más información sobre el cmdlet desinstalar ADServiceAccount, en el módulo de Active Directory para Windows PowerShell símbolo del sistema, escribe **Get-Help desinstalar-ADServiceAccount**y, a continuación, presiona ENTRAR o ver la información en la web de TechNet en [desinstalar ADServiceAccount](https://technet.microsoft.com/library/ee617202.aspx).



## <a name="BKMK_Links"></a>Consulta también

-   [Introducción a las cuentas servicio administrado grupo](group-managed-service-accounts-overview.md)




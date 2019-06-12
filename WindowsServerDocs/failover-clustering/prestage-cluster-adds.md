---
title: Preconfiguración de objetos de equipo del clúster en Active Directory Domain Services
description: Cómo preconfigurar objetos de equipo del clúster en Active Directory Domain Services.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.manager: daveba
ms.technology: storage-failover-clustering
ms.date: 05/09/2019
ms.localizationpriority: medium
ms.openlocfilehash: fa240ba5fedd98f16639dd19fb8f22c10bfdd9ac
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442462"
---
# <a name="prestage-cluster-computer-objects-in-active-directory-domain-services"></a>Preconfiguración de objetos de equipo del clúster en Active Directory Domain Services

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se muestra el modo de preconfigurar objetos de equipo en clúster en los Servicios de dominio de Active Directory (AD DS). Puede usar este procedimiento con el fin de habilitar a un usuario o grupo para que cree un clúster de conmutación por error cuando no tenga permisos para crear objetos de equipo en AD DS.

Al crear un clúster de conmutación por error con el Asistente para crear clúster o con Windows PowerShell, se debe especificar el nombre del clúster. Si dispone de suficientes permisos al crear el clúster, el proceso de creación del clúster genera automáticamente un objeto de equipo en AD DS que se corresponde con el nombre del clúster. Este objeto se denomina *objeto de nombre de clúster* o CNO. Por medio del CNO, se crean automáticamente objetos de equipo virtual (VCO) cuando se configuran roles en clúster que emplean puntos de acceso cliente. Por ejemplo, si se crea un servidor de archivos de alta disponibilidad con un punto de acceso cliente denominado *ServidorDeArchivos1*, el CNO creará un VCO correspondiente en AD DS.

>[!NOTE]
>No hay la opción para crear un clúster desconectado de Active Directory, donde no se crean ningún CNO o VCO en AD DS. Dicha opción está pensada para determinados tipos de implementaciones de clúster. Para obtener más información, consulte [Implementar un clúster desconectado de Active Directory](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v%3dws.11)>).

Para crear el CNO de forma automática, el usuario que crea el clúster de conmutación por error debe tener el permiso **Crear objetos de equipo** en la unidad organizativa o el contenedor donde residen los servidores que conformarán el clúster. Para que un usuario o grupo que no posee este permiso pueda crear un clúster, un usuario con los permisos correspondientes en AD DS (por lo general, un administrador de dominios) puede preconfigurar el CNO en AD DS. Este procedimiento también ofrece al administrador de dominios más control sobre la convención de nomenclatura empleada para el clúster y sobre la unidad organizativa en la que se crean los objetos de clúster.

## <a name="step-1-prestage-the-cno-in-ad-ds"></a>Paso 1: Preconfigurar el CNO en AD DS

Antes de comenzar, asegúrese de que conoce los siguientes datos:

- El nombre que desea asignar al clúster
- El nombre de la cuenta de usuario o grupo al que desea conceder derechos para crear el clúster

Como práctica recomendada, se aconseja crear una unidad organizativa para los objetos de clúster. Si ya existe una unidad organizativa que quiere usar, el requisito mínimo para llevar a cabo este paso es la pertenencia al grupo **Opers. de cuentas**. Si necesita crear una unidad organizativa para los objetos de clúster, el requisito mínimo para llevar a cabo este paso es la pertenencia al grupo **Admins. del dominio** u otro equivalente.

>[!NOTE]
>Si crea el CNO en el contenedor Equipos predeterminado en lugar de en una unidad organizativa, puede omitir el Paso 3 de este tema. En este punto, un administrador de clústeres puede crear un máximo de 10 VCO sin ninguna configuración adicional.

### <a name="prestage-the-cno-in-ad-ds"></a>Preconfigurar el CNO en AD DS

1. En un equipo en el que se hayan instalado las Herramientas de AD DS a partir de las Herramientas de administración remota del servidor, o en un controlador de dominio, abra **Usuarios y equipos de Active Directory**. Para hacer esto en un servidor, inicie el administrador del servidor y, a continuación, en el **herramientas** menú, seleccione **equipos y usuarios de Active Directory**.
2. Para crear una unidad organizativa para el clúster de objetos de equipo, haga clic en el nombre de dominio o una unidad organizativa existente, seleccione **New**y, a continuación, seleccione **unidad organizativa**. En el **nombre** cuadro, escriba el nombre de la unidad organizativa y, a continuación, seleccione **Aceptar**.
3. En el árbol de consola, haga clic en la unidad organizativa donde desea crear el CNO, seleccione **New**y, a continuación, seleccione **equipo**.
4. En el **nombre_equipo** , escriba el nombre que se usará para el clúster de conmutación por error y, a continuación, seleccione **Aceptar**.

   >[!NOTE]
   >Este es el nombre del clúster que el usuario que crea el clúster indicará en la página **Punto de acceso para administrar el clúster** del Asistente para crear clúster o como valor del parámetro *–Name* del cmdlet **New-Cluster** de Windows PowerShell.

5. Como práctica recomendada, haga clic en la cuenta de equipo que acaba de crear, seleccione **propiedades**y, a continuación, seleccione el **objeto** ficha. En el **objeto** ficha, seleccione el **proteger objeto contra eliminación accidental** casilla de verificación y, a continuación, seleccione **Aceptar**.
6. Haga clic en la cuenta de equipo recién creado y, a continuación, seleccione **deshabilitar cuenta**. Seleccione **Sí** para confirmar y, a continuación, seleccione **Aceptar**.

   >[!NOTE]
   >Debe deshabilitar la cuenta para que, durante la creación del clúster, el proceso de creación del clúster pueda confirmar que no hay ningún equipo o clúster en el dominio que esté utilizando la cuenta.

![CNO deshabilitado en la unidad organizativa de clústeres usada como ejemplo.](media/prestage-cluster-adds/disabled-cno-in-the-example-clusters-ou.png)

**Figura 1. CNO deshabilitado en el ejemplo de unidad organizativa de clústeres**

## <a name="step-2-grant-the-user-permissions-to-create-the-cluster"></a>Paso 2: Conceda los permisos de usuario para crear el clúster

Debe configurar los permisos de manera que la cuenta de usuario que se utilizará para crear el clúster de conmutación por error tenga permisos de control total con respecto al CNO.

El requisito mínimo para completar este paso es la pertenencia al grupo **Opers. de cuentas** .

Aquí le mostramos cómo conceder los permisos de usuario para crear el clúster:

1. En Usuarios y equipos de Active Directory, en el menú **Ver**, seleccione la opción **Características avanzadas**.
2. Busque y, a continuación, haga clic en el CNO y, a continuación, seleccione **propiedades**.
3. En el **seguridad** ficha, seleccione **agregar**.
4. En el **Seleccionar usuarios, equipos o grupos** diálogo cuadro, especifique la cuenta de usuario o grupo que desea conceder permisos a y, a continuación, seleccione **Aceptar**.
5. Seleccione la cuenta de usuario o el grupo recién agregado y, junto a **Control total**, active la casilla **Permitir** .
  
   ![Concesión de control total al usuario o grupo que creará el clúster](media/prestage-cluster-adds/granting-full-control-to-the-user-create-the-cluster.png)
  
   **Figura 2. Conceder Control total al usuario o grupo que va a crear el clúster**
6. Seleccione **Aceptar**.

Tras finalizar este paso, el usuario al que concedió los permisos puede crear el clúster de conmutación por error. Sin embargo, si el CNO está ubicado en una unidad organizativa, el usuario no puede crear roles en clúster que requieran un punto de acceso cliente hasta que se haya realizado el Paso 3.

>[!NOTE]
>Si el CNO se encuentra en el contenedor Equipos predeterminado, un administrador de clústeres puede crear un máximo de 10 VCO sin ninguna configuración adicional. Para agregar más de 10 VCO, debe conceder de forma explícita el permiso **Crear objeto de equipo** al CNO para el contenedor Equipos.

## <a name="step-3-grant-the-cno-permissions-to-the-ou-or-prestage-vcos-for-clustered-roles"></a>Paso 3: Conceda los permisos de CNO a la unidad organizativa o preconfigurar los VCO para los roles en clúster

Al crear un rol en clúster con un punto de acceso cliente, el clúster crea un VCO en la misma unidad organizativa que el CNO. Para que esto se produzca automáticamente, el CNO debe poseer permisos para crear objetos de equipo en la unidad organizativa.

Si ha preconfigurado el CNO en AD DS, tiene dos opciones para crear los VCO:

- Opción 1: [Conceda los permisos de CNO a la unidad organizativa](#grant-the-cno-permissions-to-the-ou). Con esta opción, el clúster puede crear automáticamente los VCO en AD DS. De este modo, un administrador del clúster de conmutación por error puede crear roles en clúster sin tener que solicitarle que preconfigure los VCO en AD DS.

>[!NOTE]
>El requisito mínimo para completar los pasos de esta opción es la pertenencia al grupo **Admins. del dominio** u otro equivalente.

- Opción 2: [Preconfigurar un VCO para un rol en clúster](#prestage-a-vco-for-a-clustered-role). Use esta opción si es necesario preconfigurar cuentas de roles en clúster debido a los requisitos existentes en su organización. Por ejemplo, es posible que desee controlar la convención de nomenclatura o bien controlar los roles de clúster que se crean.

>[!NOTE]
>El requisito mínimo para completar los pasos de esta opción es la pertenencia al grupo **Opers. de cuentas**.

### <a name="grant-the-cno-permissions-to-the-ou"></a>Conceda los permisos de CNO a la unidad organizativa

1. En Usuarios y equipos de Active Directory, en el menú **Ver**, seleccione la opción **Características avanzadas**.
2. Haga clic en la unidad organizativa donde creó el CNO en [paso 1: Preconfigurar el CNO en AD DS](#step-1-prestage-the-cno-in-ad-ds)y, a continuación, seleccione **propiedades**.
3. En el **seguridad** ficha, seleccione **avanzadas**.
4. En el **configuración de seguridad avanzada** cuadro de diálogo, seleccione **agregar**.
5. Junto a **Principal**, seleccione **seleccionar una entidad de seguridad**.
6. En el **Seleccionar usuario, equipo, cuenta de servicio o grupos** cuadro de diálogo, seleccione **tipos de objeto**, seleccione el **equipos** casilla de verificación y, a continuación, seleccione **Aceptar** .
7. En **escriba los nombres de objeto para seleccionar**, escriba el nombre del CNO, seleccione **comprobar nombres**y, a continuación, seleccione **Aceptar**. En respuesta al mensaje de advertencia que indica que está a punto de agregar un objeto deshabilitado, seleccione **Aceptar**.
8. En el cuadro de diálogo **Entrada de permiso**, asegúrese de que la lista **Tipo** esté definida en **Permitir** y que la lista **Se aplica a** esté definida en **Este objeto y todos los descendientes**.
9. En **Permisos**, active la casilla **Crear objetos de equipo**.

   ![Concesión del permiso Crear objetos de equipo al CNO](media/prestage-cluster-adds/granting-create-computer-objects-permission-to-the-cno.png)

   **Figura 3. Conceder el permiso Crear equipo objetos al CNO**
10. Seleccione **Aceptar** hasta volver a los usuarios de Active Directory y complemento en los equipos.

Un administrador del clúster de conmutación por error puede ahora crear roles en clúster con puntos de acceso cliente y poner los recursos en línea.

### <a name="prestage-a-vco-for-a-clustered-role"></a>Preconfigurar un VCO para un rol en clúster

1. Antes de comenzar, asegúrese de que sabe el nombre que tendrán el clúster y el rol en clúster.
2. En Usuarios y equipos de Active Directory, en el menú **Ver**, seleccione la opción **Características avanzadas**.
3. En usuarios de Active Directory y los equipos, haga clic en la unidad organizativa donde el CNO reside en el clúster, seleccione **New**y, a continuación, seleccione **equipo**.
4. En el **nombre_equipo** , escriba el nombre que usará para el rol en clúster y, a continuación, seleccione **Aceptar**.
5. Como práctica recomendada, haga clic en la cuenta de equipo que acaba de crear, seleccione **propiedades**y, a continuación, seleccione el **objeto** ficha. En el **objeto** ficha, seleccione el **proteger objeto contra eliminación accidental** casilla de verificación y, a continuación, seleccione **Aceptar**.
6. Haga clic en la cuenta de equipo recién creado y, a continuación, seleccione **propiedades**.
7. En el **seguridad** ficha, seleccione **agregar**.
8. En el **Seleccionar usuario, equipo, cuenta de servicio o grupos** cuadro de diálogo, seleccione **tipos de objeto**, seleccione el **equipos** casilla de verificación y, a continuación, seleccione **Aceptar** .
9. En **escriba los nombres de objeto para seleccionar**, escriba el nombre del CNO, seleccione **comprobar nombres**y, a continuación, seleccione **Aceptar**. Si recibe un mensaje de advertencia que indica que está a punto de agregar un objeto deshabilitado, seleccione **Aceptar**.
10. Asegúrese de que el CNO esté seleccionado y, a continuación, junto a **Control total**, active la casilla **Permitir**.
11. Seleccione **Aceptar**.

Un administrador del clúster de conmutación por error puede ahora crear el rol en clúster con un punto de acceso cliente con coincida con el nombre del VCO preconfigurado y poner los recursos en línea.

## <a name="more-information"></a>Más información

- [Clúster de conmutación por error](failover-clustering.md)
- [Configuración de cuentas de clúster en Active Directory](configure-ad-accounts.md)
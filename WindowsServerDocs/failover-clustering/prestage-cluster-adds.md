---
title: Preconfigurar los objetos de equipo de clúster en los servicios de dominio de Active Directory
description: Cómo preconfigurar los objetos de equipo de clúster en los servicios de dominio de Active Directory.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/25/2018
ms.localizationpriority: medium
ms.openlocfilehash: 111969b074b33764dbbf72bfb24ad606f8314e41
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082646"
---
# <a name="prestage-cluster-computer-objects-in-active-directory-domain-services"></a>Preconfigurar los objetos de equipo de clúster en los servicios de dominio de Active Directory

>Se aplica a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

En este tema se muestra cómo preconfigurar los objetos de equipo de clúster en los servicios de dominio de Active Directory (AD DS). Puede usar este procedimiento para habilitar un usuario o grupo crear un clúster de conmutación por error cuando no tienen permisos para crear objetos de equipo en AD DS.

Cuando se crea un clúster de conmutación por error mediante el Asistente para crear clúster o mediante el uso de Windows PowerShell, debe especificar un nombre para el clúster. Si tiene permisos suficientes al crear el clúster, el proceso de creación de clúster crea automáticamente un objeto de equipo en AD DS que coincide con el nombre del clúster. El *objeto de nombre de clúster* o el CNO llama a este objeto. A través del CNO, los objetos de equipo virtual (VCO) se crean automáticamente al configurar funciones agrupados en clúster que utilizan puntos de acceso de cliente. Por ejemplo, si crea un servidor de archivos de alta disponibilidad con un punto de acceso de cliente que se denomina *FileServer1*, el CNO creará un VCO correspondiente en AD DS.

>[!NOTE]
>En Windows Server 2012 R2, hay la opción para crear un clúster de desasociar de Active Directory, donde no se crea CNO o VCO en AD DS. Está destinado a tipos específicos de las implementaciones de clúster. Para obtener más información, vea [implementar un clúster de Active Directory-Detached](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v%3dws.11)>).

Para crear el CNO automáticamente, el usuario que crea el clúster de conmutación por error debe tener el permiso **Crear equipo objetos** a la unidad organizativa (OU) o el contenedor donde residen los servidores que formarán el clúster. Para habilitar un usuario o grupo crear un clúster sin tener este permiso, un usuario con los permisos adecuados en AD DS (normalmente, un administrador de dominio) puede preconfigurar el CNO en AD DS. Esto también proporciona al administrador de dominio más control sobre la convención de nomenclatura que se usa para el clúster y control sobre qué unidad organizativa se crean los objetos del clúster en.

## <a name="step-1-prestage-the-cno-in-ad-ds"></a>Paso 1: Preconfigurar el CNO en AD DS

Antes de empezar, asegúrese de que sabe lo siguiente:

- El nombre que desea asignar el clúster
- El nombre de la cuenta de usuario o grupo al que desea conceder derechos para crear el clúster

Como procedimiento recomendado, se recomienda que cree una unidad organizativa para los objetos del clúster. Si una unidad organizativa no existe que desea usar, pertenecer al grupo **Operadores de cuentas** es el requisito mínimo para completar este paso. Si necesita crear una unidad organizativa para los objetos de clúster, la pertenencia al grupo **Administradores de dominio** , o equivalente, es el requisito mínimo para completar este paso.

>[!NOTE]
>Si crea el CNO en el contenedor equipos predeterminado en lugar de una unidad organizativa, no es necesario que completar el paso 3 de este tema. En este escenario, un administrador de clústeres puede crear hasta 10 VCO sin ninguna configuración adicional.

### <a name="prestage-the-cno-in-ad-ds"></a>Preconfigurar el CNO en AD DS

1. En un equipo que tenga instalado desde las herramientas de administración de servidor remoto la herramientas de AD DS, o en un controlador de dominio, abra **usuarios y equipos de usuarios de Active Directory**. Para hacer esto en un servidor, inicie el Administrador de servidor y, a continuación, en el menú **Herramientas** , seleccione **equipos y usuarios de Active Directory**.
2. Para crear una unidad organizativa para el clúster de objetos de equipo, haga clic en el nombre de dominio o una unidad organizativa existente, elija **nuevo**y, a continuación, seleccione **Unidad organizativa**. En el cuadro **nombre** , escriba el nombre de la unidad organizativa y, a continuación, seleccione **Aceptar**.
3. En el árbol de consola, haga clic en la unidad organizativa donde desea crear el CNO, elija **nuevo**y, a continuación, seleccione el **equipo**.
4. En el cuadro **nombre de equipo** , escriba el nombre que se usará para el clúster de conmutación por error y, a continuación, seleccione **Aceptar**.

  >[!NOTE]
  >Este es el nombre de clúster que el usuario que crea el clúster se especifica en la página de **Punto de acceso para administrar el clúster** en el Asistente para crear clúster o como el valor de la *– nombre* parámetro para el **Nuevo clúster** Windows PowerShell cmdlet.

5. Como procedimiento recomendado, haga clic en la cuenta de equipo que acaba de crear, seleccione **Propiedades**y, a continuación, seleccione la ficha **objeto** . En la ficha **objeto** , active la casilla de verificación **Proteger objeto contra la eliminación accidental** y, a continuación, seleccione **Aceptar**.
6. Haga clic en la cuenta de equipo que acaba de crear y, a continuación, seleccione **Deshabilitar cuenta**. Seleccione **Sí** para confirmar y, a continuación, seleccione **Aceptar**.

  >[!NOTE]
  >Debe deshabilitar la cuenta de modo que durante la creación del clúster, el proceso de creación de clúster puede confirmar que la cuenta no está actualmente en uso por un equipo existente o un clúster en el dominio.

![CNO deshabilitado en la unidad organizativa de los clústeres de ejemplo](media/prestage-cluster-adds/disabled-cno-in-the-example-clusters-ou.png)

**Figura 1. CNO deshabilitado en la unidad organizativa de los clústeres de ejemplo**

## <a name="step-2-grant-the-user-permissions-to-create-the-cluster"></a>Paso 2: Conceder los permisos de usuario para crear el clúster

Debe configurar los permisos para que la cuenta de usuario que se usará para crear el clúster de conmutación por error tenga permisos de Control total para el CNO.

Pertenencia al grupo **Operadores de cuentas** es el requisito mínimo para completar este paso.

Aquí es cómo conceder los permisos de usuario para crear el clúster:

1. En usuarios de Active Directory y los equipos, en el menú **Ver** , asegúrese de que esté seleccionado **Características avanzadas** .
2. Busque y, a continuación, haga clic en el CNO y, a continuación, seleccione **Propiedades**.
3. En la ficha **seguridad** , seleccione **Agregar**.
4. En el cuadro de diálogo **Seleccionar usuarios, equipos o grupos** , especifique la cuenta de usuario o grupo que desea conceder permisos a y, a continuación, seleccione **Aceptar**.
5. Seleccione la cuenta de usuario o grupo que acaba de agregar y, a continuación, junto a **control total**, seleccione la casilla de verificación **Permitir** .
  
  ![Conceder Control total para el usuario o grupo que va a crear el clúster](media/prestage-cluster-adds/granting-full-control-to-the-user-create-the-cluster.png)
  
  **Figura 2. Conceder Control total para el usuario o grupo que va a crear el clúster**
6. Selecciona **Aceptar**.

Después de completar este paso, el usuario que se conceden permisos a puede crear el clúster de conmutación por error. Sin embargo, si el CNO se encuentra en una unidad organizativa, el usuario no puede crear roles agrupados que requieren un punto de acceso de cliente hasta que complete el paso 3.

>[!NOTE]
>Si el CNO se encuentra en el contenedor de equipos de forma predeterminada, un administrador de clústeres puede crear hasta 10 VCO sin ninguna configuración adicional. Para agregar más de 10 VCO, debe conceder explícitamente el permiso **Crear equipo objetos** al CNO para el contenedor equipos.

## <a name="step-3-grant-the-cno-permissions-to-the-ou-or-prestage-vcos-for-clustered-roles"></a>Paso 3: Conceder los permisos de CNO a la unidad organizativa o preconfigurar VCO para roles agrupados en clúster

Cuando se crea un rol agrupado con un punto de acceso de cliente, el clúster crea un VCO en la misma unidad organizativa que el CNO. Para que esto se produzca automáticamente, el CNO debe tener permisos para crear objetos de equipo en la unidad organizativa.

Si había preconfigurado el CNO en AD DS, puede hacer cualquiera de las siguientes opciones para crear VCO:

- Opción 1: [conceder los permisos de CNO a la unidad organizativa](#grant-the-cno-permissions-to-the-ou). Si utiliza esta opción, el clúster puede crear automáticamente VCO en AD DS. Por lo tanto, un administrador para el clúster de conmutación por error puede crear roles agrupados sin tener que solicitar que preconfigurar VCO en AD DS.

>[!NOTE]
>Pertenencia al grupo **Administradores de dominio** , o equivalente, es el requisito mínimo para completar los pasos para esta opción.

- Opción 2: [Prestage un VCO para un rol agrupado](#prestage-a-vco-for-the-clustered-role). Use esta opción si es necesario preconfigurar cuentas para funciones agrupadas debido a los requisitos de la organización. Por ejemplo, es posible que desee controlar la convención de nomenclatura, o el control que se crean las funciones del clúster.

>[!NOTE]
>Pertenencia al grupo **Operadores de cuentas** es el requisito mínimo para completar los pasos para esta opción.

### <a name="grant-the-cno-permissions-to-the-ou"></a>Conceder los permisos de CNO a la unidad organizativa

1. En usuarios de Active Directory y los equipos, en el menú **Ver** , asegúrese de que esté seleccionado **Características avanzadas** .
2. Haga clic en la unidad organizativa donde creó el CNO en [paso 1: preconfigurar el CNO en AD DS](#step-1:-prestage-the-CNO-in-ad-ds)y, a continuación, seleccione **Propiedades**.
3. En la ficha **seguridad** , seleccione **Opciones avanzadas**.
4. En el cuadro de diálogo **Configuración de seguridad avanzada** , seleccione **Agregar**.
5. Situada junto a la **entidad de seguridad**, seleccione **Seleccionar una entidad de seguridad**.
6. En el cuadro de diálogo **Seleccionar usuario, equipo, la cuenta de servicio o grupos** , seleccione los **Tipos de objeto**, active la casilla de verificación **equipos** y, a continuación, seleccione **Aceptar**.
7. En **Escriba los nombres de objeto que desea seleccionar**, escriba el nombre del CNO, seleccione **Comprobar nombres**y, a continuación, seleccione **Aceptar**. En respuesta al mensaje de advertencia que dice que va a agregar un objeto deshabilitado, seleccione **Aceptar**.
8. En el cuadro de diálogo **Entrada de permiso** , asegúrese de que la lista de **tipo** está establecida en **Permitir**, y se establece la lista **se aplica** a **este objeto y todos los descendientes**.
9. En **permisos**, seleccione la casilla de verificación **Crear equipo objetos** .

  ![La concesión del permiso de objetos de equipo crear al CNO](media/prestage-cluster-adds/granting-create-computer-objects-permission-to-the-cno.png)

  **Figura 3. La concesión del permiso de objetos de equipo crear al CNO**
10. Seleccione **Aceptar** hasta que vuelva a los usuarios de Active Directory y complemento de equipos.

Un administrador en el clúster de conmutación por error ahora puede crear roles agrupados con el cliente de puntos de acceso y poner los recursos en línea.

### <a name="prestage-a-vco-for-a-clustered-role"></a>Preconfigurar un VCO para un rol agrupados en clúster

1. Antes de empezar, asegúrese de que conoce el nombre del clúster y el nombre que se va a tener el rol de agrupado.
2. En usuarios de Active Directory y los equipos, en el menú **Ver** , asegúrese de que esté seleccionado **Características avanzadas** .
3. En y equipos de usuarios de Active Directory, haga clic en la unidad organizativa donde reside el CNO para el clúster, elija **nuevo**y, a continuación, seleccione el **equipo**.
4. En el cuadro **nombre de equipo** , escriba el nombre que usará para la función agrupada y, a continuación, seleccione **Aceptar**.
5. Como procedimiento recomendado, haga clic en la cuenta de equipo que acaba de crear, seleccione **Propiedades**y, a continuación, seleccione la ficha **objeto** . En la ficha **objeto** , active la casilla de verificación **Proteger objeto contra la eliminación accidental** y, a continuación, seleccione **Aceptar**.
6. Haga clic en la cuenta de equipo que acaba de crear y, a continuación, seleccione **Propiedades**.
7. En la ficha **seguridad** , seleccione **Agregar**.
8. En el cuadro de diálogo **Seleccionar usuario, equipo, la cuenta de servicio o grupos** , seleccione los **Tipos de objeto**, active la casilla de verificación **equipos** y, a continuación, seleccione **Aceptar**.
9. En **Escriba los nombres de objeto que desea seleccionar**, escriba el nombre del CNO, seleccione **Comprobar nombres**y, a continuación, seleccione **Aceptar**. Si recibe un mensaje de advertencia que dice que va a agregar un objeto deshabilitado, haga **clic en Aceptar**.
10. Asegúrese de que el CNO está seleccionado y, a continuación, junto a **control total**, seleccione la casilla de verificación **Permitir** .
11. Selecciona **Aceptar**.

Un administrador en el clúster de conmutación por error ahora puede crear la función agrupada con un punto de acceso de cliente que coincide con el nombre VCO ensayados previamente y conectar el recurso.

## <a name="more-information"></a>Más información

- [Clúster de conmutación por error](failover-clustering.md)
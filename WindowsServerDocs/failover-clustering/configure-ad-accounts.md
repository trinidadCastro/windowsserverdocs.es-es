---
title: Configuración de cuentas de clúster en Active Directory
ms.date: 11/12/2012
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 3cc7449c8fbbad2ed4a3cd27513fcbe74b617e36
ms.sourcegitcommit: 23a6e83b688119c9357262b6815c9402c2965472
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69560516"
---
# <a name="configuring-cluster-accounts-in-active-directory"></a>Configuración de cuentas de clúster en Active Directory


Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

En Windows Server, cuando se crea un clúster de conmutación por error y se configuran servicios o aplicaciones en clúster, los asistentes para clúster de conmutación por error crean las cuentas de equipo Active Directory necesarias (también denominadas objetos de equipo) y les conceden permisos específicos. Los asistentes crean una cuenta de equipo para el clúster propiamente dicho (esta cuenta también se denomina el objeto de nombre de clúster o CNO) y una cuenta de equipo para la mayoría de los tipos de servicios y aplicaciones en clúster, siendo la excepción una máquina virtual de Hyper-V. Los asistentes para clúster de conmutación por error establecen automáticamente los permisos para estas cuentas. Si se cambian los permisos, habrá que volver a cambiarlos para que coincidan con los requisitos de clúster. En esta guía se describen estas cuentas y permisos de Active Directory, se proporciona información general sobre por qué son importantes, y se describen los pasos para configurar y administrar las cuentas.
      

## <a name="overview-of-active-directory-accounts-needed-by-a-failover-cluster"></a>Información general de las cuentas de Active Directory que necesita un clúster de conmutación por error

En esta sección se describen las cuentas de equipo de Active Directory (también denominadas objetos de equipo de Active Directory) que son importantes para un clúster de conmutación por error. Estas cuentas son las siguientes:

  - **Cuenta de usuario usada para crear el clúster.** Es la cuenta de usuario utilizada para iniciar el Asistente para crear clúster. La cuenta es importante porque proporciona la base a partir de la cual se crea una cuenta de equipo para el propio clúster.  
      
  - **Cuenta de nombre de clúster.** (la cuenta de equipo del propio clúster, también denominada objeto de nombre de clúster o CNO). Esta cuenta la crea automáticamente el Asistente para crear clúster y tiene el mismo nombre que el clúster. La cuenta de nombre de clúster es muy importante, ya que a través de esta cuenta, se crean automáticamente otras cuentas a medida que configura nuevos servicios y aplicaciones en el clúster. Si se elimina la cuenta de nombre de clúster o se quitan de ella permisos, no se pueden crear otras cuentas como necesita el clúster, hasta que se restaure la cuenta de nombre de clúster o se reintegren los permisos correctos.  
      
    Por ejemplo, si crea un clúster denominado Clúster1 e intenta configurar un servidor de impresión en clúster denominado ServidorImpresión1 en el clúster, la cuenta Clúster1 de Active Directory necesitará conservar los permisos correctos para que se pueda utilizar con el fin de crear una cuenta de equipo denominada ServidorImpresión1.  
      
    La cuenta de nombre de clúster se crea en el contenedor predeterminado para las cuentas de equipo en Active Directory. De forma predeterminada, es el contenedor "Equipos", pero el administrador de dominio puede elegir redirigirlo a otro contenedor o unidad organizativa (OU).  
      
  - **La cuenta de equipo (objeto de equipo) de un servicio o aplicación en clúster.** El Asistente para alta disponibilidad crea automáticamente estas cuentas como parte del proceso de creación de la mayoría de los tipos de servicios o aplicaciones en clúster, a excepción de una máquina virtual de Hyper-V. A la cuenta de nombre de clúster se le conceden los permisos necesarios para controlar estas cuentas.  
      
    Por ejemplo, si tiene un clúster denominado Clúster1 y después crea un servidor de archivos en clúster denominado ServidorDeArchivos1, el Asistente para alta disponibilidad creará una cuenta de equipo de Active Directory denominada ServidorDeArchivos1. El Asistente para alta disponibilidad también proporciona a la cuenta Cluster1 los permisos necesarios para controlar la cuenta FileServer1.  
      

En la tabla siguiente se describen los permisos necesarios para estas cuentas.


<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Cuenta</th>
<th>Detalles sobre los permisos</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Cuenta utilizada para crear el clúster</p></td>
<td><p>Necesita permisos administrativos en los servidores que se convertirán en nodos de clúster. También necesita los permisos <strong>Crear objetos de equipo</strong> y <strong>Leer todas las propiedades</strong> en el contenedor que se utiliza para las cuentas de equipo en el dominio.</p></td>
</tr>
<tr class="even">
<td><p>Cuenta de nombre de clúster (cuenta de equipo del propio clúster)</p></td>
<td><p>Cuando se ejecuta el Asistente para crear clúster, crea la cuenta de nombre de clúster en el contenedor predeterminado que se utiliza para las cuentas de equipo en el dominio. De forma predeterminada, la cuenta de nombre de clúster (como otras cuentas de equipo) puede crear hasta diez cuentas de equipo en el dominio.</p>
<p>Si crea la cuenta de nombre de clúster (objeto de nombre de clúster) antes de crear el clúster (es decir, preconfigura la cuenta), debe concederle los permisos <strong>Crear objetos de equipo</strong> y <strong>Leer todas las propiedades</strong> en el contenedor que se utiliza para las cuentas de equipo en el dominio. También debe deshabilitar la cuenta y asignar <strong>Control total</strong> a la cuenta que utilizará el administrador que instala el clúster. Para obtener más información, vea <a href="#steps-for-prestaging-the-cluster-name-account" data-raw-source="[Steps for prestaging the cluster name account](#steps-for-prestaging-the-cluster-name-account)">Pasos para preconfigurar la cuenta de nombre de clúster</a>, más adelante en esta guía.</p></td>
</tr>
<tr class="odd">
<td><p>Cuenta de equipo de un servicio o aplicación en clúster</p></td>
<td><p>Cuando se ejecuta el Asistente para alta disponibilidad (para crear un nuevo servicio o aplicación en clúster), en la mayoría de los casos se crea una cuenta de equipo para el servicio o aplicación en clúster en Active Directory. A la cuenta de nombre de clúster se le conceden los permisos necesarios para controlar esta cuenta. La excepción es una máquina virtual de Hyper-V en clúster: no se crea ninguna cuenta de equipo para este.</p>
<p>Si preconfigura la cuenta de equipo para un servicio o aplicación en clúster, debe configurarla con los permisos necesarios. Para obtener más información, vea <a href="#steps-for-prestaging-an-account-for-a-clustered-service-or-application" data-raw-source="[Steps for prestaging an account for a clustered service or application](#steps-for-prestaging-an-account-for-a-clustered-service-or-application)">Pasos para preconfigurar una cuenta para un servicio o una aplicación en clúster</a>, más adelante en esta guía.</p></td>
</tr>
</tbody>
</table>


> [!NOTE]
> En versiones anteriores de Windows Server, había una cuenta para el Servicio de clúster. Sin embargo Servicio de clúster, dado que Windows Server 2008 se ejecuta automáticamente en un contexto especial que proporciona los permisos y privilegios específicos necesarios para el servicio (similar al contexto de sistema local, pero con privilegios reducidos). Sin embargo, se necesitan otras cuentas como se describe en esta guía. 
<br>


### <a name="how-accounts-are-created-through-wizards-in-failover-clustering"></a>Cómo se crean cuentas mediante asistentes en los clústeres de conmutación por error

En el diagrama siguiente se muestra el uso y la creación de cuentas de equipo (objetos de Active Directory) que se describen en la subsección anterior. Estas cuentas entran en juego cuando un administrador ejecuta el Asistente para crear clúster y, a continuación, ejecuta el Asistente para alta disponibilidad (para configurar un servicio o una aplicación en clúster).

![](media/configure-ad-accounts/Cc731002.e8a7686c-9ba8-4ddf-87b1-175b7b51f65d(WS.10).gif)

Tenga en cuenta que el diagrama anterior muestra un único administrador que ejecuta el Asistente para crear clúster y el Asistente para alta disponibilidad. Sin embargo, podrían ser dos administradores diferentes que usaran dos cuentas de usuario distintas, si ambas cuentas tuvieran permisos suficientes. Los permisos se describen con más detalle en requisitos relacionados con los clústeres de conmutación por error, los dominios de Active Directory y las cuentas, más adelante en esta guía.

### <a name="how-problems-can-result-if-accounts-needed-by-the-cluster-are-changed"></a>Qué problemas pueden producirse si se cambian las cuentas que necesita el clúster

En el diagrama siguiente se muestra qué problemas pueden producirse si se cambia la cuenta de nombre de clúster (una de las cuentas que necesita el clúster) después de haberla creado automáticamente el Asistente para crear clúster.

![](media/configure-ad-accounts/Cc731002.beecc4f7-049c-4945-8fad-2cceafd6a4a5(WS.10).gif)

Si se produce el tipo de problema mostrado en el diagrama, se registrará un evento determinado (1193, 1194, 1206 ó 1207) en el Visor de eventos. Para obtener más información acerca de estos eventos [http://go.microsoft.com/fwlink/?LinkId=118271](http://go.microsoft.com/fwlink/?linkid=118271), vea.

Tenga en cuenta que se puede producir un problema similar con la creación de una cuenta para un servicio o una aplicación en clúster si se ha alcanzado la cuota para todo el dominio respecto a la creación de objetos de equipo (de forma predeterminada, 10). En tal caso, podría ser adecuado consultar con el administrador del dominio cómo aumentar la cuota, aunque se trata de una configuración para todo el dominio y solo se debe modificar después de haberlo estudiado detenidamente y solo después de confirmar que el diagrama anterior no describe su situación. Para obtener más información, vea [Pasos para solucionar problemas debidos a cambios en las cuentas de Active Directory relacionadas con el clúster](#steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts), más adelante en esta guía.

## <a name="requirements-related-to-failover-clusters-active-directory-domains-and-accounts"></a>Requisitos relacionados con los clústeres de conmutación por error, dominios de Active Directory y cuentas

Tal y como se describe en las tres secciones anteriores, se deben cumplir ciertos requisitos antes de poder configurar correctamente los servicios y las aplicaciones en clúster en un clúster de conmutación por error. Los requisitos más básicos se refieren a la ubicación de los nodos de clúster (dentro de un único dominio) y el nivel de permisos de la cuenta de la persona que instala el clúster. Si se cumplen estos requisitos, los asistentes para clúster de conmutación por error pueden crear automáticamente las demás cuentas requeridas por el clúster. En la lista siguiente se proporcionan detalles sobre estos requisitos básicos.

  - **Nodos** todos los nodos deben estar en el mismo dominio de Active Directory. (El dominio no puede estar basado en Windows NT 4.0, que no incluye Active Directory.)  
      
  - **Cuenta de la persona que instala el clúster:** la persona que instala el clúster debe utilizar una cuenta con las siguientes características:  
      
      - La cuenta debe ser una cuenta de dominio. No tiene por qué ser una cuenta de administrador de dominio. Puede ser una cuenta de usuario del dominio si cumple los demás requisitos de esta lista.  
          
      - La cuenta debe tener permisos administrativos en los servidores que se convertirán en nodos de clúster. La manera más sencilla de proporcionar esto es crear una cuenta de usuario de dominio y, a continuación, agregar esa cuenta al grupo local Administradores en cada uno de los servidores que se convertirán en nodos de clúster. Para obtener más información, vea [Pasos para configurar la cuenta para la persona que instala el clúster](#steps-for-configuring-the-account-for-the-person-who-installs-the-cluster), más adelante en esta guía.  
          
      - La cuenta (o el grupo del que la cuenta es miembro) debe tener concedidos los permisos **Crear objetos de equipo** y **Leer todas las propiedades** en el contenedor que se utiliza para las cuentas de equipo del dominio. Otra alternativa es convertir la cuenta en una cuenta de administrador de dominio. Para obtener más información, vea [Pasos para configurar la cuenta para la persona que instala el clúster](#steps-for-configuring-the-account-for-the-person-who-installs-the-cluster), más adelante en esta guía.  
          
      - Si su organización elige preconfigurar la cuenta de nombre de clúster (una cuenta de equipo con el mismo nombre que el clúster), la cuenta de nombre de clúster preconfigurada debe conceder el permiso "Control total" a la cuenta de la persona que instala el clúster. Para obtener otros detalles importantes sobre cómo preconfigurar la cuenta de nombre de clúster, vea [Pasos para preconfigurar la cuenta de nombre de clúster](#steps-for-prestaging-the-cluster-name-account), más adelante en esta guía.  
          

### <a name="planning-ahead-for-password-resets-and-other-account-maintenance"></a>Planear de antemano los restablecimientos de la contraseña y otras tareas de mantenimiento de la cuenta

A veces, los administradores de clústeres de conmutación por error pueden necesitar restablecer la contraseña de la cuenta de nombre de clúster. Esta acción necesita un permiso concreto, el permiso **Restablecer contraseña**. Por tanto, es una práctica recomendada editar los permisos de la cuenta de nombre de clúster (utilizando el complemento Usuarios y equipos de Active Directory) para conceder el permiso **Restablecer contraseña** a los administradores del clúster para la cuenta de nombre de clúster. Para obtener más información, vea [Pasos para solucionar problemas de contraseña con la cuenta de nombre de clúster](#steps-for-troubleshooting-password-problems-with-the-cluster-name-account), más adelante en esta guía.

## <a name="steps-for-configuring-the-account-for-the-person-who-installs-the-cluster"></a>Pasos para configurar la cuenta para la persona que instala el clúster

La cuenta de la persona que instala el clúster es importante porque proporciona la base a partir de la cual se crea una cuenta de equipo para el propio clúster.

La pertenencia a grupos mínima necesaria para completar el procedimiento siguiente depende de si está creando la cuenta de dominio y asignándole los permisos necesarios en el dominio, o si solo está poniendo la cuenta (creada por otra persona) en el grupo local **Administradores** en los servidores que serán nodos del clúster de conmutación por error. En el primer caso, la pertenencia a **Opers. de cuentas** o **Admins. del dominio**, o equivalente, es el mínimo necesario para completar este procedimiento. En el segundo caso, la pertenencia al grupo local **Administradores** de los servidores que serán nodos del clúster de conmutación por error, o un equivalente, es todo lo necesario. Revise los detalles sobre el uso de las cuentas adecuadas y [http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477)las pertenencias a grupos en.

#### <a name="to-configure-the-account-for-the-person-who-installs-the-cluster"></a>Para configurar la cuenta para la persona que instala el clúster

1.  Cree u obtenga una cuenta de dominio para la persona que instala el clúster. Esta cuenta puede ser una cuenta de usuario de dominio o una cuenta de administrador de dominio (en **Admins. del dominio** o un grupo equivalente).

2.  Si la cuenta que se creó u obtuvo en el paso 1 es una cuenta de usuario de dominio, o si las cuentas de administrador de dominio de su dominio no se incluyen automáticamente en el grupo local **Administradores** en los equipos del dominio, agregue la cuenta al grupo local **Administradores** en los servidores que serán nodos del clúster de conmutación por error:
    
    1.  Haga clic en **Inicio**, **Herramientas administrativas** y **Administrador del servidor**.  
          
    2.  En el árbol de consola, expanda sucesivamente **Configuración**, **Usuarios y grupos locales**, y **Grupos**.  
          
    3.  En el panel central, haga clic con el botón secundario en **Administradores**, haga clic en **Agregar a grupo** y, a continuación, haga clic en **Agregar**.  
          
    4.  En **Escriba los nombres de objeto que desea seleccionar**, escriba el nombre de la cuenta de usuario que se creó u obtuvo en el paso 1. Si se le solicita, escriba un nombre de cuenta y una contraseña con permisos suficientes para esta acción. A continuación, haga clic en **Aceptar**.  
          
    5.  Repita estos pasos en cada servidor que será un nodo del clúster de conmutación por error.  
          
    

> [!IMPORTANT]
> Estos pasos se deben repetir en todos los servidores que serán nodos del clúster. 
<br>


3. Si la cuenta que se creó u obtuvo en el paso 1 es una cuenta de administrador de dominio, omita el resto de este procedimiento. De lo contrario, conceda a la cuenta los permisos **Crear objetos de equipo** y **Leer todas las propiedades** en el contenedor que se utiliza para las cuentas de equipo en el dominio:
    
   1.  En un controlador de dominio, haga clic en **Inicio**, haga clic en **Herramientas administrativas** y, a continuación, haga clic en **Usuarios y equipos de Active Directory**. Si aparece el cuadro de diálogo **Control de cuentas de usuario**, confirme que la acción que muestra es la que desea y, a continuación, haga clic en **Continuar**.  
          
   2.  En el menú **Ver**, asegúrese de que está seleccionado **Características avanzadas**.  
          
       Cuando se ha seleccionado **Características avanzadas**, puede ver la pestaña **Seguridad** en las propiedades de cuentas (objetos) en **Usuarios y equipos de Active Directory**.  
          
   3.  Haga clic con el botón secundario en el contenedor predeterminado **Equipos** o en el contenedor predeterminado donde se crean las cuentas de equipo en su dominio y, a continuación, haga clic en **Propiedades**. **Equipos** se encuentra en <b>Active Directory usuarios y equipos/</b><i>dominio-nodo</i><b>/equipos</b>.  
          
   4.  En la pestaña **Seguridad** , haga clic en **Opciones avanzadas**.  
          
   5.  Haga clic en **Agregar**, escriba el nombre de la cuenta que se creó u obtuvo en el paso 1 y, a continuación, haga clic en **Aceptar**.  
          
   6.  En el cuadro de diálogo **contenedor de entrada de permiso para * * ** , busque los permisos **crear objetos de equipo** y **leer todas las propiedades** y asegúrese de que la casilla **permitir** está activada para cada uno.  
          
       ![](media/configure-ad-accounts/Cc731002.0a863ac5-2024-4f9f-8a4d-a419aff32fa0(WS.10).gif)

## <a name="steps-for-prestaging-the-cluster-name-account"></a>Pasos para preconfigurar la cuenta de nombre de clúster

Normalmente es más sencillo si no preconfigura la cuenta de nombre de clúster, sino que permite crear y configurar automáticamente la cuenta al ejecutar el Asistente para crear clúster. Sin embargo, si es necesario preconfigurar la cuenta de nombre de clúster debido a los requisitos de su organización, utilice el procedimiento siguiente.

El requisito mínimo para completar este procedimiento es la pertenencia al grupo **Admins. del dominio** u otro equivalente. Revise los detalles sobre el uso de las cuentas adecuadas y [http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477)las pertenencias a grupos en. Observe que para este procedimiento puede utilizar la misma cuenta que usará al crear el clúster.

#### <a name="to-prestage-a-cluster-name-account"></a>Para preconfigurar una cuenta de nombre de clúster

1.  Asegúrese de que sabe el nombre que tendrá el clúster y el nombre de la cuenta de usuario que utilizará la persona que crea el clúster. (Puede utilizar esa cuenta para realizar este procedimiento.)

2.  En un controlador de dominio, haga clic en **Inicio**, haga clic en **Herramientas administrativas** y, a continuación, haga clic en **Usuarios y equipos de Active Directory**. Si aparece el cuadro de diálogo **Control de cuentas de usuario**, confirme que la acción que muestra es la que desea y, a continuación, haga clic en **Continuar**.

3.  En el árbol de consola, haga clic con el botón secundario en **Equipos** o en el contenedor predeterminado donde se crean las cuentas de equipo en su dominio. **Equipos** se encuentra en <b>Active Directory usuarios y equipos/</b><i>dominio-nodo</i><b>/equipos</b>.

4.  Haga clic en **Nuevo** y, a continuación, haga clic en **Equipo**.

5.  Escriba el nombre que se utilizará para el clúster de conmutación por error, es decir, el nombre del clúster que se especificará en el Asistente para crear clúster y, a continuación, haga clic en **Aceptar**.

6.  Haga clic con el botón secundario en la cuenta recién creada y, a continuación, haga clic en **Deshabilitar cuenta**. Si se le pide confirmación de la opción, haga clic en **Sí**.
    
    Se debe deshabilitar la cuenta para que cuando se ejecute el asistente **Crear clúster**, pueda confirmar que la cuenta que utilizará para el clúster no está actualmente en uso por un equipo o clúster existente en el dominio.

7.  En el menú **Ver**, asegúrese de que está seleccionado **Características avanzadas**.
    
    Cuando se ha seleccionado **Características avanzadas**, puede ver la pestaña **Seguridad** en las propiedades de cuentas (objetos) en **Usuarios y equipos de Active Directory**.

8.  Haga clic con el botón secundario en la carpeta en la que hizo clic con el botón secundario en el paso 3 y, a continuación, haga clic en **propiedades**.

9.  En la pestaña **Seguridad** , haga clic en **Opciones avanzadas**.

10. Haga clic en **Agregar**, haga clic en **Tipos de objetos** y asegúrese de que se ha seleccionado **Equipos**; a continuación, haga clic en **Aceptar**. Después, en **Escriba el nombre de objeto para seleccionar**, escriba el nombre de la cuenta de equipo recién creada y, a continuación, haga clic en **Aceptar**. Si aparece un mensaje que indica que está a punto de agregar un objeto deshabilitado, haga clic en **Aceptar**.

11. En el cuadro de diálogo **Entrada de permiso**, busque los permisos **Crear objetos de equipo** y **Leer todas las propiedades**, y asegúrese de que la casilla **Permitir** está activada para cada uno.
    
    ![](media/configure-ad-accounts/Cc731002.f5977c4d-a62e-4b17-81e3-8c19ddca2078(WS.10).gif)

12. Haga clic en **Aceptar** hasta volver al complemento **Usuarios y equipos de Active Directory**.

13. Si está utilizando la misma cuenta para realizar este procedimiento que se utilizará para crear el clúster, omita los pasos restantes. De lo contrario, debe configurar los permisos de manera que la cuenta de usuario que se utilizará para crear el clúster tenga control total de la cuenta de equipo recién creada:
    
    1.  En el menú **Ver**, asegúrese de que está seleccionado **Características avanzadas**.  
          
    2.  Haga clic con el botón secundario en la cuenta de equipo recién creada y, a continuación, haga clic en **Propiedades**.  
          
    3.  En la pestaña **Seguridad**, haga clic en **Agregar**. Si aparece el cuadro de diálogo **Control de cuentas de usuario**, confirme que la acción que muestra es la que desea y, a continuación, haga clic en **Continuar**.  
          
    4.  Utilice el cuadro de diálogo **Seleccionar usuarios, equipos o grupos** para especificar la cuenta de usuario que se utilizará al crear el clúster. A continuación, haga clic en **Aceptar**.  
          
    5.  Asegúrese de que la cuenta de usuario recién agregada está seleccionada y a continuación, junto a **Control total**, active la casilla **Permitir**.  
          
        ![](media/configure-ad-accounts/Cc731002.fffaafe2-a494-498b-974c-8f9d70f7103b(WS.10).gif)

## <a name="steps-for-prestaging-an-account-for-a-clustered-service-or-application"></a>Pasos para preconfigurar una cuenta para un servicio o una aplicación en clúster

Normalmente es más sencillo si no preconfigura la cuenta de equipo para un servicio o una aplicación en clúster, sino que permite crear y configurar automáticamente la cuenta al ejecutar el Asistente para alta disponibilidad. Sin embargo, si es necesario preconfigurar cuentas debido a los requisitos de su organización, utilice el procedimiento siguiente.

El requisito mínimo para completar este procedimiento es la pertenencia al grupo **Opers. de cuentas** u otro equivalente. Revise los detalles sobre el uso de las cuentas adecuadas y [http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477)las pertenencias a grupos en.

#### <a name="to-prestage-an-account-for-a-clustered-service-or-application"></a>Para preconfigurar una cuenta para un servicio o una aplicación en clúster

1.  Asegúrese de que sabe el nombre que tendrán el clúster y el servicio o la aplicación en clúster.

2.  En un controlador de dominio, haga clic en **Inicio**, haga clic en **Herramientas administrativas** y, a continuación, haga clic en **Usuarios y equipos de Active Directory**. Si aparece el cuadro de diálogo **Control de cuentas de usuario**, confirme que la acción que muestra es la que desea y, a continuación, haga clic en **Continuar**.

3.  En el árbol de consola, haga clic con el botón secundario en **Equipos** o en el contenedor predeterminado donde se crean las cuentas de equipo en su dominio. **Equipos** se encuentra en <b>Active Directory usuarios y equipos/</b><i>dominio-nodo</i><b>/equipos</b>.

4.  Haga clic en **Nuevo** y, a continuación, haga clic en **Equipo**.

5.  Escriba el nombre que utilizará para el servicio o la aplicación en clúster y, a continuación, haga clic en **Aceptar**.

6.  En el menú **Ver**, asegúrese de que está seleccionado **Características avanzadas**.
    
    Cuando se ha seleccionado **Características avanzadas**, puede ver la pestaña **Seguridad** en las propiedades de cuentas (objetos) en **Usuarios y equipos de Active Directory**.

7.  Haga clic con el botón secundario en la cuenta de equipo recién creada y, a continuación, haga clic en **Propiedades**.

8.  En la pestaña **Seguridad**, haga clic en **Agregar**.

9.  Haga clic en **Tipos de objeto**, asegúrese de que se ha seleccionad **Equipos** y, a continuación, haga clic en **Aceptar**. Después, en **Escriba el nombre de objeto para seleccionar**, escriba la cuenta de nombre de clúster y, a continuación, haga clic en **Aceptar**. Si aparece un mensaje que indica que está a punto de agregar un objeto deshabilitado, haga clic en **Aceptar**.

10. Asegúrese de que la cuenta de nombre de clúster está seleccionada y a continuación, junto a **Control total**, active la casilla **Permitir**.
    
    ![](media/configure-ad-accounts/Cc731002.2e614376-87a6-453a-81ba-90ff7ebc3fa2(WS.10).gif)

## <a name="steps-for-troubleshooting-problems-related-to-accounts-used-by-the-cluster"></a>Pasos para solucionar problemas relacionados con las cuentas utilizadas por el clúster

Como se ha descrito anteriormente en esta guía, al crear un clúster de conmutación por error y configurar servicios o aplicaciones en clúster, los asistentes para clúster de conmutación por error crean las cuentas de Active Directory necesarias y les conceden los permisos correctos. Si se elimina una cuenta necesaria, o se cambian permisos necesarios, pueden producirse problemas. En las subsecciones siguientes se proporcionan pasos para solucionar estos problemas.

  - [Pasos para solucionar problemas de contraseña con la cuenta de nombre de clúster](#steps-for-troubleshooting-password-problems-with-the-cluster-name-account)
      
  - [Pasos para solucionar problemas causados por cambios en las cuentas de Active Directory relacionadas con el clúster](#steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts)
      

### <a name="steps-for-troubleshooting-password-problems-with-the-cluster-name-account"></a>Pasos para solucionar problemas de contraseña con la cuenta de nombre de clúster

Utilice este procedimiento si hay un mensaje de evento sobre los objetos de equipo o sobre la identidad del clúster que incluye el texto siguiente. Tenga en cuenta que este texto estará dentro del mensaje de evento, no al principio del mensaje de evento:

`Logon failure: unknown user name or bad password.`

Los mensajes de evento que se ajustan a la descripción anterior indican que la contraseña para la cuenta de nombre de clúster y la contraseña correspondiente almacenada por el software de clúster ya no coinciden.

Para obtener información acerca de cómo asegurarse de que los administradores de clústeres tienen los permisos correctos para realizar el siguiente procedimiento según sea necesario, consulte planeación previa de restablecimientos de contraseña y otro mantenimiento de cuentas, anteriormente en esta guía.

Para completar este procedimiento, se requiere como mínimo la pertenencia al grupo local **Administradores** o equivalente. Además, se debe conceder a su cuenta el permiso **Restablecer contraseña** para la cuenta de nombre de clúster (a menos que sea una cuenta **Admins. del dominio** o sea el creador propietario de la cuenta de nombre de clúster). Se puede utilizar para este procedimiento la cuenta empleada por la persona que instaló el clúster. Revise los detalles sobre el uso de las cuentas adecuadas y [http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477)las pertenencias a grupos en.

#### <a name="to-troubleshoot-password-problems-with-the-cluster-name-account"></a>Para solucionar problemas de contraseña con la cuenta de nombre de clúster

1.  Para abrir el complemento del clúster de conmutación por error, haga clic en **Inicio**, en **Herramientas administrativas** y, a continuación, haga clic en **Administración de clúster de conmutación por error**. (Si aparece el cuadro de diálogo **control de cuentas de usuario** , confirme que la acción que se muestra es la que desea y, a continuación, haga clic en **continuar**).

2.  En el complemento Administración del clúster de conmutación por error, si no aparece el clúster que desea configurar, en el árbol de consola, haga clic con el botón secundario en **Administración del clúster de conmutación por error**, haga clic **Administrar un clúster** y, a continuación, seleccione o especifique el clúster que desea.

3.  En el panel central, expanda **Recursos principales de clúster**.

4.  En **Nombre del clúster**, haga clic con el botón secundario en el elemento **Nombre**, seleccione **Acciones adicionales** y, a continuación, haga clic en **Reparar objeto de Active Directory**.

### <a name="steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts"></a>Pasos para solucionar problemas debidos a cambios en las cuentas de Active Directory relacionadas con el clúster

Si se elimina la cuenta de nombre de clúster o se quitan permisos de ella, se producirán problemas cuando intente configurar un nuevo servicio o aplicación en clúster. Para solucionar un problema en el que esta podría ser la causa, utilice el complemento Usuarios y equipos de Active Directory para ver o cambiar la cuenta de nombre de clúster y otras cuentas relacionadas. Para obtener información acerca de los eventos que se registran cuando se produce este tipo de problema (evento 1193, 1194, 1206 o 1207 [http://go.microsoft.com/fwlink/?LinkId=118271](http://go.microsoft.com/fwlink/?linkid=118271)), vea.

El requisito mínimo para completar este procedimiento es la pertenencia al grupo **Admins. del dominio** u otro equivalente. Revise los detalles sobre el uso de las cuentas adecuadas y [http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477)las pertenencias a grupos en.

#### <a name="to-troubleshoot-problems-caused-by-changes-in-cluster-related-active-directory-accounts"></a>Para solucionar problemas debidos a cambios en las cuentas de Active Directory relacionadas con el clúster

1.  En un controlador de dominio, haga clic en **Inicio**, haga clic en **Herramientas administrativas** y, a continuación, haga clic en **Usuarios y equipos de Active Directory**. Si aparece el cuadro de diálogo **Control de cuentas de usuario**, confirme que la acción que muestra es la que desea y, a continuación, haga clic en **Continuar**.

2.  Expanda el contenedor predeterminado **Equipos** o la carpeta donde se encuentra la cuenta de nombre de clúster (la cuenta de equipo del clúster). **Equipos** se encuentra en <b>Active Directory usuarios y equipos/</b><i>dominio-nodo</i><b>/equipos</b>.

3.  Examine el icono de la cuenta de nombre de clúster. No debe tener una flecha que señale hacia abajo; es decir, la cuenta no debe estar deshabilitada. Si parece estar deshabilitada, haga clic con el botón secundario en ella y busque el comando **Habilitar cuenta**. Si ve el comando, haga clic en él.

4.  En el menú **Ver**, asegúrese de que está seleccionado **Características avanzadas**.
    
    Cuando se ha seleccionado **Características avanzadas**, puede ver la pestaña **Seguridad** en las propiedades de cuentas (objetos) en **Usuarios y equipos de Active Directory**.

5.  Haga clic con el botón secundario en el contenedor predeterminado **Equipos** o en la carpeta donde se encuentra la cuenta de nombre de clúster.

6.  Haga clic en **Propiedades**.

7.  En la pestaña **Seguridad** , haga clic en **Opciones avanzadas**.

8.  En la lista de cuentas con permisos, haga clic en la cuenta de nombre de clúster y, a continuación, haga clic en **Editar**.
    

> [!NOTE]
> Si la cuenta de nombre de clúster no aparece, hace clic <STRONG>Agregar</STRONG> y agréguela a la lista. 
<br>


9. Para la cuenta de nombre de clúster (también conocida como objeto de nombre de clúster o CNO), asegúrese de que está seleccionado **Permitir** para los permisos **Crear objetos de equipo** y **Leer todas las propiedades**.
    
   ![](media/configure-ad-accounts/Cc731002.f5977c4d-a62e-4b17-81e3-8c19ddca2078(WS.10).gif)

10. Haga clic en **Aceptar** hasta volver al complemento **Usuarios y equipos de Active Directory**.

11. Examine las directivas de dominio (consultando con un administrador de dominio si es aplicable) relacionadas con la creación de cuentas de equipo (objetos). Asegúrese de que la cuenta de nombre de clúster puede crear una cuenta de equipo cada vez que configura un servicio o una aplicación en clúster. Por ejemplo, si su administrador de dominio ha configurado opciones que harán que todas las cuentas de equipo nuevas se creen en un contenedor especializado en lugar de en el contenedor predeterminado **Equipos**, asegúrese de que esta configuración permita a la cuenta de nombre de clúster crear también nuevas cuentas de equipo en ese contenedor.

12. Expanda el contenedor predeterminado **Equipos** o el contenedor donde se encuentra la cuenta de equipo para uno de los servicios o aplicaciones en clúster.

13. Haga clic con el botón secundario en la cuenta de equipo para uno de los servicios o aplicaciones en clúster y, a continuación, haga clic en **Propiedades**.

14. En la pestaña **Seguridad**, confirme que la cuenta de nombre de clúster figura entre las cuentas que tienen permisos y selecciónela. Confirme que la cuenta de nombre de clúster tiene el permiso **Control total** (la casilla **Permitir** está activada). Si no lo tiene, agregue la cuenta de nombre de clúster a la lista y concédala el permiso **Control total**.
    
   ![](media/configure-ad-accounts/Cc731002.2e614376-87a6-453a-81ba-90ff7ebc3fa2(WS.10).gif)

15. Repita los pasos 13 y 14 para cada servicio y aplicación en clúster configurado en el clúster.

16. Compruebe que no se ha alcanzado la cuota para todo el dominio para la creación de objetos de equipo (de forma predeterminada, 10), consulte con un administrador de dominio si es aplicable. Si todos los elementos anteriores de este procedimiento se han revisado y corregido, y si se ha alcanzado la cuota, considere la posibilidad de aumentar la cuota. Para cambiar la cuota:
    
   1.  Abra un símbolo del sistema como administrador y ejecute **ADSIEdit.msc**.  
          
   2.  Haga clic con el botón secundario en **Editor ADSI**, haga clic en **Conectar con** y, a continuación, haga clic en **Aceptar**. El **Contexto de nomenclatura predeterminado** se agregará al árbol de consola.  
          
   3.  Haga doble clic en **Contexto de nomenclatura predeterminado**, haga clic con el botón secundario en el objeto de dominio que hay debajo de él y, a continuación, haga clic en **Propiedades**.  
          
   4.  Vaya hasta **ms-DS-MachineAccountQuota**, selecciónelo, haga clic en **Editar**, cambie el valor y, a continuación, haga clic en **Aceptar**.


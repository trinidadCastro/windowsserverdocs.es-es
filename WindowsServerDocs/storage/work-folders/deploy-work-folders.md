---
ms.assetid: d2429185-9720-4a04-ad94-e89a9350cdba
title: Implementar Carpetas de trabajo
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
author: JasonGerend
manager: dongill
ms.author: jgerend
ms.date: 6/24/2017
description: Cómo implementar Carpetas de trabajo, incluyendo la instalación del rol de servidor, la creación de recursos compartidos de sincronización y la creación de registros DNS.
ms.openlocfilehash: d2ba117a021cfc7361c0f7c8df2ed9f3c4bc9d94
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67792341"
---
# <a name="deploying-work-folders"></a>Implementar Carpetas de trabajo

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10, Windows 8.1, Windows 7

En este tema se describen los pasos necesarios para implementar Carpetas de trabajo. Se da por supuesto que ya has leído [Planificar una implementación de Carpetas de trabajo](plan-work-folders.md).  
  
 Para implementar Carpetas de trabajo, un proceso que puede implicar la participación de varios servidores y tecnologías, utilice los pasos siguientes.  
  
> [!TIP]
>  La implementación más simple de carpetas de trabajo es un único servidor de archivos (a menudo llamado servidor de sincronización) sin soporte de sincronización a través de Internet que puede ser una implementación útil para un laboratorio de pruebas o una solución de sincronización para los equipos cliente unidos a un dominio. Para crear una implementación simple, estos son los pasos mínimos que deben seguirse: 
>  -   Paso 1: obtener un certificado SSL  
>  -   Paso 2: Crear registros de DNS 
>  -   Paso 3: instalar Carpetas de trabajo en los servidores de archivos  
>  -   Paso 4: Enlazar el certificado SSL en los servidores de sincronización
>  -   Paso 5: Crear grupos de seguridad para carpetas de trabajo  
>  -   Paso 7: crear recursos compartidos de sincronización para los datos del usuario  
  
## <a name="step-1-obtain-ssl-certificates"></a>Paso 1: obtener un certificado SSL  
 Carpetas de trabajo usa HTTPS para sincronizar de forma segura archivos entre los clientes de Carpetas de trabajo y el servidor de Carpetas de trabajo. Los requisitos de los certificados SSL utilizados por Carpetas de trabajo son los siguientes:  
  
- El certificado debe emitirlo una entidad de certificación de confianza. Para la mayoría de implementaciones de Carpetas de trabajo, se recomienda una CA de confianza pública, ya que los certificados los utilizarán dispositivos basados en Internet, no unidos a dominios.  
  
- El certificado debe ser válido.  
  
- La clave privada del certificado debe ser exportable (ya que deberá instalar el certificado en varios servidores).  
  
- El nombre del firmante del certificado debe contener la URL de Carpetas de trabajo utilizada para descubrir el servicio Carpetas de trabajo por Internet; debe tener el formato `workfolders.` *<domain_name>* .  
  
- El certificado debe incluir nombres alternativos de firmante (SAN) y enumerar el nombre del servidor de cada servidor de sincronización en uso.

  El [blog](https://blogs.technet.microsoft.com/filecab/2013/08/09/work-folders-certificate-management/) Work Folders Certificate Management proporciona información adicional sobre el uso de certificados con Carpetas de trabajo.
  
## <a name="step-2-create-dns-records"></a>Paso 2: Crear registros de DNS  
 Para que los usuarios puedan realizar la sincronización por Internet, debe crear un registro Host (A) en el DNS público para que los clientes de Internet puedan resolver la URL de Carpetas de trabajo. La resolución del registro de DNS debe ser la interfaz externa del servidor proxy inverso.  
  
 En la red interna, debes crear un registro CNAME en DNS denominado "workfolders" y que se resuelva en el nombre de dominio completo (FDQN) de un servidor de Carpetas de trabajo. Cuando los clientes de carpetas de trabajo usan la detección automática, la dirección URL que se usa para detectar el servidor de carpetas de trabajo es https:\//workfolders.domain.com. Si tienes previsto usar la detección automática, el registro CNAME denominado "workfolders" debe existir en DNS.  
  
## <a name="step-3-install-work-folders-on-file-servers"></a>Paso 3: instalar Carpetas de trabajo en los servidores de archivos  
 Puede instalar Carpetas de trabajo en un servidor unido a un dominio con el Administrador del servidor o con Windows PowerShell, localmente o de forma remota a través de una red. Esto resulta útil si está configurando varios servidores de sincronización por su red.  
  
Para implementar el rol en el Administrador del servidor, lleve a cabo lo siguiente:  
  
1.  Inicie el **Asistente para agregar roles y características**.  
  
2.  En la página **Seleccionar tipo de instalación** , elija **Instalación basada en características o en roles**.  
  
3.  En la página **Seleccionar servidor de destino** , seleccione el servidor en el cual desea instalar Carpetas de trabajo.  
  
4.  En la página **Seleccionar roles de servidor**, expanda **Servicios de archivos y almacenamiento**, expanda **Servicios de iSCSI y archivo** y posteriormente seleccione **Carpetas de trabajo**.  
  
5.  Cuando se le solicite si desea instalar el **Núcleo de web hospedable de IIS**, haga clic en **Aceptar** para instalar la versión mínima de Internet Information Services (IIS) requerida por Carpetas de trabajo.  
  
6.  Haga clic en **Siguiente** hasta que haya completado el asistente.

Para implementar el rol con Windows PowerShell, utilice el cmdlet siguiente:
  
```powershell  
Add-WindowsFeature FS-SyncShareService  
```

## <a name="step-4-binding-the-ssl-certificate-on-the-sync-servers"></a>Paso 4: Enlazar el certificado SSL en los servidores de sincronización
 Carpetas de trabajo instala el Núcleo de web hospedable de IIS, que es un componente de IIS diseñado para habilitar los servicios web sin que sea necesario realizar una instalación completa de IIS. Tras instalar el Núcleo de web hospedable de IIS, debe enlazar el certificado IIS para el servidor al sitio web predeterminado en el servidor de archivos. No obstante, el Núcleo de web hospedable de IIS no instala la Consola de administración de IIS.

 Existen dos opciones para enlazar el certificado a la interfaz web predeterminada. Para usar cualquiera de las opciones, debes tener instalada la clave privada del certificado en el almacén personal del equipo.

- Utilice la Consola de administración de IIS en un servidor que la tenga instalada. Desde la consola, conéctese al servidor de archivos que desea administrar y posteriormente seleccione el sitio web predeterminado para el servidor. El sitio web predeterminado aparecerá deshabilitado, pero usted seguirá pudiendo editar los vínculos para el sitio y seleccionar el certificado para enlazarlo al sitio web.

- Utilice el comando netsh para enlazar el certificado a la interfaz https del sitio web predeterminado. El comando es el siguiente:

    ```
    netsh http add sslcert ipport=<IP address>:443 certhash=<Cert thumbprint> appid={CE66697B-3AA0-49D1-BDBD-A25C8359FD5D} certstorename=MY
    ```

## <a name="step-5-create-security-groups-for-work-folders"></a>Paso 5: Crear grupos de seguridad para carpetas de trabajo
 Antes de crear recursos compartidos de sincronización, un miembro de los grupos Admins. del dominio o Administradores de organización deben crear grupos de seguridad en los Servicios de dominio de Active Directory (AD DS) para Carpetas de trabajo (también deben delegar algún control según se describe en el paso 6). A continuación se detallan los grupos necesarios:

- Un grupo por recurso compartido de sincronización para especificar qué usuarios tienen permiso para sincronizarse con el recurso compartido de sincronización

- Un grupo para todos los administradores de Carpetas de trabajo, de modo que puedan editar un atributo en cada objeto del usuario que vincule el usuario con el servidor de sincronización correcto (en el caso de que uses varios servidores de sincronización).

  Los grupos deben seguir una convención de nomenclatura estándar y deben utilizarse únicamente para Carpetas de trabajo, de cara a evitar posibles conflictos con otros requisitos de seguridad.

  Para crear los grupos de seguridad adecuados, utilice el procedimiento siguiente varias veces (una vez para cada recurso compartido de sincronización y una vez para crear, de forma opcional, un grupo para administradores de servidores de archivos).

#### <a name="to-create-security-groups-for-work-folders"></a>Para crear grupos de seguridad para Carpetas de trabajo

1. Abre el Administrador del servidor en un equipo que ejecute Windows Server 2012 R2 o Windows Server 2016 y que tenga instalado el Centro de administración de Active Directory.

2.  En el menú **Herramientas** , haga clic en **Centro de administración de Active Directory**. Aparece el Centro de administración de Active Directory.

3.  Haga clic con el botón derecho en el contenedor en el cual desea crear el grupo nuevo (por ejemplo, el contenedor Usuarios de la unidad organizativa o el dominio adecuados), haga clic en **Nuevo** y posteriormente haga clic en **Grupo**.

4.  En la ventana **Crear grupo** , en la sección **Grupo** , especifique la configuración siguiente:

    -   En **Nombre de grupo**, escribe el nombre del grupo de seguridad, como por ejemplo: **Usuarios del recurso compartido de sincronización de RRHH** o **Administradores de Carpetas de trabajo**.  
  
    -   En **Ámbito de grupo**, haga clic en **Seguridad** y posteriormente haga clic en **Global**.  
  
5.  En la sección **Miembros**, haga clic en **Agregar**. Aparece el cuadro de diálogo Seleccionar Usuarios, Contactos, Equipos, Cuentas de servicio o Grupos.  
  
6.  Escribe los nombres de los usuarios o grupos a los cuales concederás acceso a un recurso compartido de sincronización específico (si es que estás creando un grupo para controlar el acceso a un recurso compartido de sincronización) o escribe los nombres de los administradores de Carpetas de trabajo (si vas a configurar cuentas de usuario para descubrir automáticamente el servidor de sincronización adecuado); a continuación, haz clic en **Aceptar** y haz clic de nuevo en **Aceptar**.

Para crear un grupo de seguridad con Windows PowerShell, utilice los cmdlets siguientes:

```powershell  
$GroupName = "Work Folders Administrators"  
$DC = "DC1.contoso.com"  
$ADGroupPath = "CN=Users,DC=contoso,DC=com"  
$Members = "CN=Maya Bender,CN=Users,DC=contoso,DC=com","CN=Irwin Hume,CN=Users,DC=contoso,DC=com"  
  
New-ADGroup -GroupCategory:"Security" -GroupScope:"Global" -Name:$GroupName -Path:$ADGroupPath -SamAccountName:$GroupName -Server:$DC  
Set-ADGroup -Add:@{'Member'=$Members} -Identity:$GroupName -Server:$DC
```

## <a name="step-6-optionally-delegate-user-attribute-control-to-work-folders-administrators"></a>Paso 6: Delegar opcionalmente el control de los atributos de usuario a los administradores de carpetas de trabajo  
 Si estás implementando varios servidores de sincronización y quieres dirigir automáticamente a los usuarios al servidor de sincronización correcto, deberás actualizar un atributo de cada cuenta de usuario en AD DS. No obstante, esto normalmente requiere la obtención de un miembro de los grupos Admins. del dominio o Administradores de organización para actualizar los atributos, lo cual puede convertirse fácilmente en una tarea ardua si es necesario agregar usuarios con frecuencia o moverlos entre servidores de sincronización.  
  
 Por este motivo, un miembro de los grupos Admins. del dominio o Administradores de organización posiblemente querrá delegar la capacidad de modificar la propiedad msDS-SyncServerURL de los objetos de usuario al grupo Administradores de Carpetas de trabajo que hemos creado en el paso 5, según se describe en el procedimiento siguiente.  
  
#### <a name="delegate-the-ability-to-edit-the-msds-syncserverurl-property-on-user-objects-in-ad-ds"></a>Delegar la capacidad de editar la propiedad msDS-SyncServerURL de los objetos de usuario en AD DS  
  
1.  Abre el Administrador del servidor en un equipo que ejecute Windows Server 2012 R2 o Windows Server 2016 y que tenga instalado Usuarios y equipos de Active Directory.  
  
2.  En el menú **Herramientas**, haga clic en **Usuarios y equipos de Active Directory**. Aparece Usuarios y equipos de Active Directory.  
  
3.  Haga clic con el botón derecho en la unidad organizativa bajo la cual existen todos los objetos de usuario de Carpetas de trabajo (si los usuarios se almacenan en varias unidades organizativas o en varios dominios) y posteriormente haga clic en **Delegar control…** . Aparece el Asistente para delegación de control.  
  
4.  En la página **Usuarios o grupos** , haga clic en **Agregar…** y posteriormente especifique el grupo que ha creado para los administradores de Carpetas de trabajo (por ejemplo, **Administradores de carpetas de trabajo**).  
  
5.  En la página **Tareas que se delegarán** , haga clic en **Crear una tarea personalizada para delegar**.  
  
6.  En la página **Tipo de objeto de Active Directory**, haga clic en **Solo los siguientes objetos de la carpeta** y posteriormente active la casilla **Objetos de usuario**.  
  
7.  En la página **Permisos**, desactive la casilla **General**, active la casilla **Específico de la propiedad** y posteriormente active las casillas **Lectura de msDS-SyncServerUrl** y **Escritura de msDS-SyncServerUrl**.

Para delegar la capacidad de editar la propiedad msDS-SyncServerURL en objetos de usuario con Windows PowerShell, utilice el script de ejemplo siguiente, que utiliza el comando DsAcls.
  
```powershell  
$GroupName = "Contoso\Work Folders Administrators"  
$ADGroupPath = "CN=Users,dc=contoso,dc=com"  
  
DsAcls $ADGroupPath /I:S /G ""$GroupName":RPWP;msDS-SyncServerUrl;user"  
```  
  
> [!NOTE]
>  La operación de delegación puede tardar un poco en ejecutarse en los dominios con un número elevado de usuarios.  
  
## <a name="step-7-create-sync-shares-for-user-data"></a>Paso 7: crear recursos compartidos de sincronización para los datos del usuario  
 En este momento, ya estás listo para designar una carpeta del servidor de sincronización para que almacene los archivos de los usuarios. Se conoce a esta carpeta como un recurso compartido de sincronización; puede utilizar los procedimientos siguientes para crear una.  
  
1. Si todavía no dispones de un volumen NTFS con espacio libre para el recurso compartido de sincronización y los archivos de usuario que contendrá, crea un volumen nuevo y dale formato con el sistema de archivos NTFS.  
  
2. En el Administrador del servidor, haga clic en **Servicios de archivos y almacenamiento**y posteriormente haga clic en **Carpetas de trabajo**.  
  
3. En la parte superior del panel de detalles se visualizará una lista de todos los recursos compartidos de sincronización existentes. Para crear un recurso compartido de sincronización nuevo, en el menú **Tareas** elija **Nuevo recurso compartido de sincronización…** . Se abre el Asistente para crear recursos compartidos de sincronización.  
  
4. En la página **Seleccionar el servidor y la ruta de acceso** , especifique la ubicación de almacenamiento del recurso compartido de sincronización. Si ya tiene un recurso compartido de archivo creado para los datos de este usuario, puede elegir este recurso compartido. Como alternativa, puede crear una carpeta nueva.  
  
   > [!NOTE]
   >  De forma predeterminada, no se puede obtener acceso directo a los recursos compartidos de sincronización mediante un recurso compartido de archivo (a menos que elijas un recurso compartido de archivo existente). Si desea hacer que un recurso compartido de sincronización sea accesible a través de un recurso compartido de archivo, utilice el icono **Recursos compartidos** del Administrador del servidor o el cmdlet [New-SmbShare](https://technet.microsoft.com/library/jj635722.aspx) para crear un recurso compartido de archivo, preferentemente con la enumeración basada en el acceso activada.  
  
5. En la página **Especificar la estructura de las carpetas de usuario**, elija una convención de nomenclatura para las carpetas de usuario del recurso compartido de sincronización. Hay dos opciones disponibles:  
  
   - **Alias de usuario** crea carpetas de usuario que no incluyan un nombre de dominio. Si está utilizando un recurso compartido de archivo que ya se utiliza con Redirección de carpetas o con otra solución de datos del usuario, seleccione esta convención de nomenclatura. Opcionalmente puede activar la casilla **Sincronizar solo la siguiente subcarpeta** para sincronizar únicamente una subcarpeta específica, como la carpeta Documentos.  
  
   - <strong>alias@domain de usuario</strong> crea carpetas de usuario que incluyan un nombre de dominio. Si no usas un recurso compartido de archivo que ya esté siendo utilizado con la opción Redirección de carpetas o con otra solución de datos del usuario, selecciona esta convención de nomenclatura para eliminar los conflictos de nomenclatura de carpetas cuando varios usuarios del recurso compartido tengan alias idénticos (lo cual puede suceder si los usuarios pertenecen a dominios distintos).  
  
6. En la página **Escriba el nombre del recurso compartido de sincronización**, especifique un nombre y una descripción para el recurso compartido de sincronización. Esto no se publicita en la red pero resulta visible en el Administrador del servidor y en Windows Powershell para ayudar a diferenciar los distintos recursos compartidos de sincronización.  
  
7. En la página **Conceder acceso a sincronización a grupos** , especifique el grupo que ha creado y que enumera los usuarios con permiso para utilizar el recurso compartido de sincronización.  
  
   > [!IMPORTANT]
   >  Para mejorar el rendimiento y la seguridad, conceda acceso a grupos en lugar de a usuarios individuales, y sea lo más específico que pueda, evitando grupos genéricos como Usuarios autenticados y Usuarios del dominio. Conceder acceso a grupos con un número elevado de usuarios aumenta el tiempo que Carpetas de trabajo necesita para consultar AD DS. Si tiene un número elevado de usuarios, cree varios recursos compartidos de sincronización para ayudar a dispersar la carga.  
  
8. En la página **Especificar directivas de dispositivos** , especifique si se solicitarán restricciones de seguridad en los dispositivos y equipos cliente. Hay dos directivas de dispositivos que pueden seleccionarse de forma individual:  
  
   -   **Cifrar Carpetas de trabajo** Solicita que Carpetas de trabajo se cifre en los dispositivos y equipos cliente  
  
   -   **Bloquear pantalla automáticamente y requerir contraseña** Solicita que los dispositivos y equipos cliente bloqueen automáticamente sus pantallas transcurridos 15 minutos, requieran una contraseña de seis o más caracteres para desbloquear la pantalla y activen un modo de bloqueo del dispositivo tras 10 intentos fallidos.  
  
       > [!IMPORTANT]
       >  Para aplicar obligatoriamente directivas de contraseña para equipos con Windows 7 y para no administradores en equipos unidos a dominios, utilice las directivas de contraseña de directiva de grupo para los dominios de equipo y excluya estos dominios de las directivas de contraseña de Carpetas de trabajo. Puede excluir dominios con el cmdlet [Set-Syncshare -PasswordAutoExcludeDomain](https://technet.microsoft.com/library/dn296649\(v=wps.630\).aspx) después de crear el recurso compartido de sincronización. Para obtener información sobre la configuración de directivas de contraseña de la directiva de grupo, consulte [Directiva de contraseña](https://technet.microsoft.com/library/hh994572(v=ws.11).aspx).  
  
9. Revise sus selecciones y complete el asistente para crear el nuevo recurso compartido de sincronización.

Puede crear recursos compartidos de sincronización con Windows PowerShell, utilizando el cmdlet [New-SyncShare](https://technet.microsoft.com/library/dn296635.aspx) . A continuación se muestra un ejemplo de este método:  
  
```powershell  
New-SyncShare "HR Sync Share" K:\Share-1 –User "HR Sync Share Users"  
```  
  
En el ejemplo anterior se crea un nuevo recurso compartido de sincronización con el nombre *Recurso compartido 01*, la ruta de acceso *K:\Recurso compartido-1* y acceso concedido al grupo con el nombre *Usuarios del recurso compartido de sincronización de RRHH*  
  
> [!TIP]
>  Tras crear recursos compartidos de sincronización, puede utilizar la funcionalidad del Administrador de recursos del servidor de archivos para administrar los datos de los recursos compartidos. Por ejemplo, puede utilizar el icono **Cuota** del interior de la página Carpetas de trabajo en el Administrador del servidor para establecer cuotas en las carpetas de usuario. Asimismo, puedes usar la [Administración del filtrado de archivos](https://technet.microsoft.com/library/cc732074.aspx) para controlar los tipos de archivos que sincronizará Carpetas de trabajo o puedes usar los escenarios descritos en [Control de acceso dinámico](https://technet.microsoft.com/windows-server-docs/identity/solution-guides/dynamic-access-control--scenario-overview) en relación con las tareas de clasificación de archivos más sofisticadas.  
  
## <a name="step-8-optionally-specify-a-tech-support-email-address"></a>Paso 8: Opcionalmente, especifique una dirección de correo electrónico de soporte técnico   
 Después de instalar Carpetas de trabajo en un servidor de archivos, es posible que quieras especificar una dirección de correo electrónico de contacto administrativo para el servidor. Usa el siguiente procedimiento para agregar una dirección de correo electrónico:  
  
#### <a name="specifying-an-administrative-contact-email"></a>Especificar un correo electrónico de contacto administrativo 
  
1.  En el Administrador del servidor, haga clic en **Servicios de archivos y almacenamiento** y posteriormente haga clic en **Servidores**.  
  
2.  Haga clic con el botón derecho en el servidor de sincronización y, a continuación, haga clic en **Configuración de Carpetas de trabajo**. Aparece la ventana Configuración de Carpetas de trabajo.  
  
3.  En el panel de navegación, haga clic en **Correo electrónico de soporte** y posteriormente escriba la dirección o las direcciones de correo electrónico que los usuarios deben utilizar cuando envíen correos electrónicos para solicitar ayuda en relación con Carpetas de soporte. Haz clic en **Aceptar** cuando hayas acabado.  
  
     Los usuarios de Carpetas de trabajo pueden hacer clic en un enlace del elemento Carpetas de trabajo del Panel de control que enviará un correo electrónico con información de diagnóstico acerca del equipo cliente a las direcciones especificadas por los usuarios.  
  
## <a name="step-9-optionally-set-up-server-automatic-discovery"></a>Paso 9: configurar opcionalmente el descubrimiento automático del servidor  
 Si está hospedando varios servidores de sincronización en su entorno, debe configurar el descubrimiento automático del servidor rellenando la propiedad **msDS-SyncServerURL** en las cuentas de usuario de AD DS.  
  
>[!NOTE]
>No se debe definir la propiedad msDS-SyncServerURL en Active Directory para usuarios remotos que tienen acceso a Carpetas de trabajo a través de una solución de proxy inverso, como por ejemplo, Proxy de aplicación web o Proxy de aplicación de Azure AD. Si se define la propiedad msDS-SyncServerURL, el cliente de Carpetas de trabajo intentará obtener acceso a una dirección URL interna que no es accesible a través de la solución de proxy inverso. Al usar el Proxy de aplicación web o el Proxy de aplicación de Azure AD, debes crear aplicaciones de proxy únicas para cada servidor de Carpetas de trabajo. Para obtener más información, consulte [implementar carpetas de trabajo con AD FS y Proxy de aplicación Web: Información general sobre](deploy-work-folders-adfs-overview.md) o [implementar carpetas de trabajo con Azure AD Application Proxy](https://blogs.technet.microsoft.com/filecab/2017/05/31/enable-remote-access-to-work-folders-using-azure-active-directory-application-proxy/).


 Para poder hacerlo, debes instalar un controlador de dominio de Windows Server 2012 R2 o actualizar los esquemas de dominio y bosque con los comandos `Adprep /forestprep` y `Adprep /domainprep`. Para obtener información acerca de cómo ejecutar con seguridad estos comandos, consulte [Ejecutar Adprep](https://technet.microsoft.com/library/dd464018.aspx).  
  
 Probablemente deseará crear un grupo de seguridad para los administradores del servidor de archivos y concederles permisos delegados para modificar este atributo de usuario específico, como se describe en el paso 5 y en el paso 6. Sin estos pasos deberá obtener un miembro del grupo Admins. del dominio o Administradores de organización para configurar el descubrimiento automático para cada usuario.  
  
#### <a name="to-specify-the-sync-server-for-users"></a>Para especificar el servidor de sincronización para los usuarios  
  
1.  Abra el Administrador del servidor en un equipo que tenga instaladas las Herramientas de administración de Active Directory.  
  
2.  En el menú **Herramientas** , haga clic en **Centro de administración de Active Directory**. Aparece el Centro de administración de Active Directory.  
  
3.  Navegue al contenedor **Usuarios** en el dominio adecuado, haga clic con el botón derecho en el usuario al cual desea asignar un recurso compartido de sincronización y posteriormente haga clic en **Propiedades**.  
  
4.  En el panel de navegación, haga clic en **Extensiones**.  
  
5.  Haga clic en la pestaña **Editor de atributos** , seleccione **msDS-SyncServerUrl** y posteriormente haga clic en **Editar**. Aparece el cuadro de diálogo Editor de cadena multivalor.  
  
6.  En el cuadro **Valor para agregar**, escriba la URL del servidor de sincronización con el cual desea que se sincronice este usuario, haga clic en **Agregar**, haga clic en **Aceptar** y posteriormente haga clic en **Aceptar** de nuevo.  
  
    > [!NOTE]
    >  La URL del servidor de sincronización es simplemente `https://` o `http://` (en función de si desea requerir una conexión segura) seguido del nombre de dominio completo del servidor de sincronización. Por ejemplo, **https:\//sync1.contoso.com**.

Para rellenar el atributo para varios usuarios, utilice Active Directory PowerShell. A continuación se proporciona un ejemplo que rellena el atributo para todos los miembros del grupo *Usuarios del recurso compartido de sincronización de RRHH* , tratado en el paso 5.
  
```powershell  
$SyncServerURL = "https://sync1.contoso.com"  
$GroupName = "HR Sync Share Users"  
  
Get-ADGroupMember -Identity $GroupName |  
Set-ADUser –Add @{"msDS-SyncServerURL"=$SyncServerURL}  
  
```  
  
## <a name="step-10-optionally-configure-web-application-proxy-azure-ad-application-proxy-or-another-reverse-proxy"></a>Paso 10: Opcionalmente, configurar Proxy de aplicación Web, Azure AD Application Proxy u otro proxy inverso  

Para permitir que los usuarios remotos tengan acceso a sus archivos, debes publicar el servidor de Carpetas de trabajo a través de un proxy inverso; gracias a ello, Carpetas de trabajo estará disponible en Internet de manera externa. Puedes usar el Proxy de aplicación web, el Proxy de aplicación de Azure Active Directory u otra solución de proxy inverso.  
  
Para configurar el acceso a Carpetas de trabajo con AD FS y el Proxy de aplicación web, consulta [Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web (WAP)](deploy-work-folders-adfs-overview.md). Para obtener información general sobre el Proxy de aplicación web, consulta [Proxy de aplicación web en Windows Server 2016](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/web-application-proxy-windows-server). Para obtener más información sobre la publicación de aplicaciones, como publicar Carpetas de trabajo en Internet mediante el Proxy de aplicación web, consulta [Publicar aplicaciones con la autenticación previa de AD FS](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/publishing-applications-using-ad-fs-preauthentication).
 
Para configurar el acceso a Carpetas de trabajo con el Proxy de aplicación de Azure Active Directory, consulta [Habilitar el acceso remoto a Carpetas de trabajo con el Proxy de aplicación de Azure Active Directory](https://blogs.technet.microsoft.com/filecab/?p=7945) 
  
## <a name="step-11-optionally-use-group-policy-to-configure-domain-joined-pcs"></a>Paso 11: utilizar opcionalmente la directiva de grupo para configurar equipos unidos a dominios  

Si tiene un número elevado de equipos unidos a dominios a los que desea implementar Carpetas de trabajo, puede utilizar la directiva de grupo para realizar las siguientes tareas de configuración del equipo cliente:  
  
- Especificar el servidor de sincronización con el cual deben sincronizarse los usuarios  
  
- Aplique la configuración automática de Carpetas de trabajo, con la configuración predeterminada (revise los aspectos tratados acerca de la directiva de grupo en [Diseñar una implementación de Carpetas de trabajo](plan-work-folders.md) antes de hacerlo)  
  
  Para controlar esta configuración, cree un nuevo objeto de directiva de grupo (GPO) para Carpetas de trabajo y posteriormente configure los valores de directiva de grupo siguientes, según corresponda:  
  
- Configuración de directiva "Especificar la configuración de Carpetas de trabajo" en Configuración del usuario\Directivas\Plantillas administrativas\Componentes de Windows\WorkFolders  
  
- Configuración de la directiva "Aplicar la configuración automática a todos los usuarios" en Configuración del equipo\Directivas\Plantillas administrativas\Componentes de Windows\WorkFolders  
  
> [!NOTE]
>  Estas configuraciones de seguridad únicamente están disponibles cuando se edita la directiva de grupo desde un equipo que ejecuta la Administración de directivas de grupo en Windows 8.1, Windows Server 2012 R2 o posterior. Las versiones de Administración de directivas de grupo de sistemas operativos anteriores no tienen esta configuración disponible. Esta configuración de directiva se aplica a equipos con Windows 7 en los que se haya instalado la aplicación [Carpetas de trabajo para Windows 7](http://blogs.technet.com/b/filecab/archive/2014/04/24/work-folders-for-windows-7.aspx) .  
  
##  <a name="BKMK_LINKS"></a> Vea también  
 Para obtener más información relacionada, vea los siguientes recursos.  
  
|Tipo de contenido|Referencias|  
|------------------|----------------|  
|**Descripción**|-   [Carpetas de trabajo](work-folders-overview.md)|  
|**Planeamiento**|-   [Diseñar una implementación de carpetas de trabajo](plan-work-folders.md)|
|**Implementación**|-   [Implementar carpetas de trabajo con AD FS y Proxy de aplicación Web (WAP)](deploy-work-folders-adfs-overview.md)<br />-   [Implementación de laboratorio de prueba de carpetas de trabajo](http://blogs.technet.com/b/filecab/archive/2013/07/10/work-folders-test-lab-deployment.aspx) (entrada de blog)<br />-   [Un nuevo atributo de usuario para la dirección Url del servidor de carpetas de trabajo](http://blogs.technet.com/b/filecab/archive/2013/10/09/a-new-user-attribute-for-work-folders-server-url.aspx) (entrada de blog)|  
|**Referencia técnica**|-   [Inicio de sesión interactivo: Umbral de bloqueo de cuenta de equipo](https://technet.microsoft.com/library/jj966264(v=ws.11).aspx)<br />-   [Cmdlets de recurso compartido de sincronización](https://docs.microsoft.com/powershell/module/syncshare/?view=win10-ps)|

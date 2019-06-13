---
ms.assetid: e6fa9069-ec9c-4615-b266-957194b49e11
title: Actualización de AD RMS a Windows Server 2016
description: ''
author: msmbaldwin
ms.author: esaggese
ms.date: 05/30/2019
ms.topic: article
ms.openlocfilehash: f5d621a0ba06f5b1beb97ccdbffb8376b5503168
ms.sourcegitcommit: 927adf32faa6052234ad08f21125906362e593dc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/12/2019
ms.locfileid: "67033342"
---
# <a name="upgrading-ad-rms-to-windows-server-2016"></a>Actualización de AD RMS a Windows Server 2016

## <a name="introduction"></a>Introducción

Active Directory Rights Management Services (AD RMS) es un servicio de Microsoft que protege los correos electrónicos y documentos confidenciales. A diferencia de los métodos tradicionales de protección, como firewalls y ACL, son persistentes con independencia de donde se pasa un archivo o cómo se transmite a protección y cifrado de AD RMS. 

Este documento proporciona orientación para migrar desde Windows Server 2012 R2 con SQL Server 2012 para Windows Server 2016 y SQL Server 2016. Puede utilizarse el mismo proceso para migrar desde versiones anteriores pero compatibles de AD RMS.
Tenga en cuenta que Active Directory Rights Management Services ya no está en desarrollo activo, y para las funcionalidades más recientes, los clientes deben considerar migrar a [Azure Information Protection](https://azure.microsoft.com/services/information-protection/), que ofrece un mucho más conjunto completo de características de dispositivo más completa y la compatibilidad de aplicaciones. 

Para obtener información sobre la migración de AD RMS a Azure Information Protection sin tener que volver a proteger su contenido, consulte [la documentación de migración de Azure Information Protection](https://docs.microsoft.com/azure/information-protection/migrate-from-ad-rms-to-azure-rms).

## <a name="about-the-environment-used-in-this-guide"></a>Acerca del entorno usado en esta guía

AD FS es un componente opcional de una instalación de AD RMS. En esta guía, se supone el uso de ADFS. Si AD FS no se ha usado en su entorno para admitir usuarios de AD RMS, puede omitir todos los pasos que hacen referencia a AD FS.

En esta guía, SQL Server se actualiza a SQL Server 2016 al realizar una instalación en paralelo y migrar las bases de datos a través de una copia de seguridad. Como alternativa, si se puede actualizar los servidores de base de datos de AD RMS y AD FS a SQL Server 2016 in situ, puede mover a la siguiente sección en este documento después de tener realizar sin tener que seguir los pasos descritos en esta sección.  

## <a name="installation"></a>Instalación

### <a name="configuring-sql-server-2016"></a>Configuración de SQL Server 2016

Las siguientes tareas de implementación de los detalles de la sección relacionados directamente con la configuración de SQL Server 2016. Esta guía se centra en usar el administrador del servidor y SQL Server Management Studio para completar estas tareas.

Estos pasos deben realizarse en una instalación de SQL Server 2016. Instalar SQL Server 2016 en el hardware adecuado según las directivas y procedimientos recomendados estándar de su organización. 

#### <a name="preparing-the-sql-server"></a>Preparación de SQL Server

La siguiente sección describe cómo preparar el servidor SQL Server para que se puede actualizar a SQL Server 2016 antes de actualizar otros servicios en la plataforma de AD RMS para usar Windows Server 2016.

##### <a name="adding-cname-for-sql-server-2016-to-dns"></a>Adición de CNAME para SQL Server 2016 en DNS

El registro CNAME se utiliza para ayudar a garantizar que el programa de instalación de Windows Server 2016 obtendrá los datos apropiados desde el se señala a la nueva versión de SQL Server 2016. **Nota: Si ya usa un registro CNAME para el servicio de AD FS y AD RMS, puede mover los pasos siguientes.**


**Para agregar un registro CNAME para SQL Server 2016 en DNS**

1.  Inicie sesión en el controlador de dominio de Windows Server 2012 R2 con las credenciales de administrador de dominio.

2.  Abra el Administrador del servidor.

3.  Haga clic en **herramientas** y seleccione **DNS** para abrir el Administrador de DNS.

4.  En el panel de navegación izquierdo, expanda el controlador de dominio y abra **zonas de búsqueda directa**.

5.  Abrir los recursos de dominio adecuado, a continuación, haga clic con el botón derecho en el panel de vista a la derecha y seleccione **nuevo Alias (CNAME)** para empezar a crear el registro CNAME.

6.  Escriba el nombre de alias en un nombre lógico para diferenciarla de otras puede estar presente (ex. SQLADRMS o SQLADFS)

7.  Después de escribir el nombre, proporcione el FQDN para el host de destino que será el nuevo servidor SQL Server 2016. (p. ej. SQL2016.contoso.com)

8.  Una vez que se ha especificado toda la información, haga clic en **Aceptar**.

##### <a name="backup-the-ad-rms-and-adfs-databases"></a>Copia de seguridad de AD RMS y las bases de datos de AD FS

Las bases de datos de AD RMS y AD FS contienen información crítica necesaria para AD RMS, como la clave pública del certificado de emisor de licencias de servidor, plantillas de directiva, los datos de configuración de AD FS e información de registro. Sin estas bases de datos, los clientes no pueden emitir licencias para consumir contenido protegido, entre otros problemas.

Las bases de datos, la base de datos de configuración de AD RMS se considera la más importante, ya que almacena el SLC, derechos de las plantillas de directiva, claves de los usuarios y la información de configuración. Por lo tanto, aunque debe tener cuidado al realizar una copia de seguridad de todas las bases de datos de AD RMS y AD FS, debe planear realizar una copia de seguridad de la base de datos de configuración con regularidad.

La base de datos de registro almacena información acerca de las solicitudes de usuario para el clúster de AD RMS para los certificados y licencias de uso. En la directiva de retención de este tipo de información de la compañía, se debe basar su estrategia de copia de seguridad de esta base de datos.

La base de datos de servicios de directorio no es crítico para la funcionalidad de AD RMS y, si se pierde los datos más recientes, la base de datos se vuelva a llenar con información como el servidor de AD RMS recibe las solicitudes de certificados y licencias de uso. No es necesario para esta base de datos de copia de seguridad con regularidad, pero debe tener al menos una copia de la base de datos tal como se configuró originalmente después de la implementación de AD RMS.

**Copia de seguridad de una base de datos de AD RMS o AD FS con Microsoft SQL Server**

1.  Inicie sesión en el servidor de base de datos de Windows Server 2012 R2 AD RMS con SQL 2012.

2.  Haga clic en **iniciar**, haga clic en **todos los programas**, haga clic en **Microsoft SQL Server**y haga clic en **SQL Server Management Studio**.

3.  En el **conectar al servidor** ventana, confirme que el servidor que hospeda las bases de datos de AD RMS está en el **nombre del servidor** y haga clic en **Connect**.

4.  Expanda **bases de datos**. A la derecha, haga clic en la base de datos adecuado (**DRM** y **Adfs**), seleccione **tareas**y seleccione **copia de seguridad**.

5.  Repita el paso 4 para las bases de datos restantes.

6.  Asegúrese de que se puede tener acceso a la copia de seguridad de las bases de datos en otras máquinas en la red o con un dispositivo de almacenamiento tal como se necesitarán para pasos posteriores durante la migración.

Ahora puede almacenar las copias de base de datos en una ubicación segura. No olvide hacer una copia de seguridad de las bases de datos con frecuencia.

##### <a name="adding-domain-admin-sql-ad-rms-andor-adfs-service-account-to-sql-server-2016"></a>Agregar administrador de dominio, cuenta SQL, AD RMS o servicio de AD FS a SQL Server 2016

Los pasos siguientes mostrarán cómo agregar varias cuentas de servicio a SQL Server 2016 para ayudarle a migrar los datos desde el entorno de Windows Server 2012 R2. Esto le proporcionará los permisos adecuados al intentar obtener acceso al contenido y administrar los datos.

**Para agregar el Administrador de dominio, SQL, AD RMS o cuenta de servicio de AD FS a SQL Server**

1.  Inicie sesión como la cuenta de administrador Local el servidor con SQL Server 2016.

2.  Haga clic en **iniciar**, haga clic en **todos los programas**, haga clic en **Microsoft SQL Server**y haga clic en **SQL Server Management Studio**.

3.  En el **conectar al servidor** ventana, confirme que el servidor que hospeda las bases de datos de AD RMS está en el **nombre del servidor** , a continuación, haga clic en el menú desplegable para la autenticación y seleccione **SQL Server Autenticación**.

4.  En el **inicio de sesión** campo escriba el nombre de la cuenta de administrador Local (p. ej. LocalAdmin) y, a continuación, proporcione la contraseña correspondiente y haga clic en **Connect**.

5.  Expanda **seguridad** y, a continuación, haga clic en **inicios de sesión** y seleccione **nuevo inicio de sesión** en el menú contextual que aparece.

6.  Una vez que aparezca la ventana, escriba en la cuenta de administrador de dominio en el **nombre de inicio de sesión** campo (p. ej. Contoso\\ContosoAdmin)

7.  En el panel de navegación izquierdo, elija **Roles de servidor**.

8.  A continuación, marque la casilla de verificación **sysadmin** según los roles de servidor y haga clic en **Aceptar**.

9.  Reiniciar **administración de SQL Server**.

10. En el **conectar al servidor** ventana, confirme que el servidor que hospeda las bases de datos de AD RMS está en el **nombre del servidor** , a continuación, haga clic en el menú desplegable para la autenticación y seleccione **Windows Autenticación** y haga clic en **Connect**.

##### <a name="restoring-the-ad-rms-and-adfs-databases-to-sql-server-2016"></a>Restauración de AD RMS y las bases de datos de AD FS a SQL Server 2016

Los pasos siguientes mostrarán cómo restaurar los datos de la instancia de SQL Server anterior a la nueva instancia de 2016. Esto permitirá que el nuevo SQL usar los datos de configuración pertinente de las bases de datos anteriores de AD RMS y AD FS.

**Para restaurar los datos del servidor SQL Server anterior al nuevo servidor SQL**

1.  Inicie sesión en el servidor con SQL Server 2016 con la cuenta apropiada.

2.  En el panel de navegación izquierdo, haga clic en **bases de datos** y seleccione **Restore Database** para comenzar el proceso de restauración.

3.  En **origen** elija **dispositivo** y, a continuación, busque la ubicación donde se almacenan los archivos de base de datos en los pasos anteriores.

4.  Una vez que se han seleccionado los archivos, haga clic en **Aceptar**.

5.  Asegúrese de que todos los archivos de base de datos se han agregado y complete el proceso haciendo **Aceptar**.

### <a name="configuring-windows-server-2016-active-directory-federation-services-ad-fs"></a>Configuración de servicios de federación de Active Directory (AD FS) de Windows Server 2016

Se ha implementado AD FS para proporcionar inicio de sesión en acceso único (SSO) para AD RMS como una aplicación. También se ha configurado con la extensión de dispositivos de Mobile (MDE) de AD RMS, que permite la compatibilidad con dispositivos móviles para los usuarios finales y Mac.

Las secciones siguientes proporcionan instrucciones sobre las tareas operativas, es posible que deba realizar en la implementación de AD FS.

#### <a name="adding-a-2016-ad-fs-server-to-the-farm"></a>Agregar un servidor de ADFS 2016 a la granja de servidores

Puede implementar servidores de AD FS adicionales para admitir la implementación de AD RMS. Puede elegir realizar esta acción si se produce un aumento de tráfico a los servidores de AD RMS, o aplicaciones adicionales, o si tiene que retirar uno de los servidores que se utiliza actualmente para AD FS.

**Para agregar el servidor 2016 AD FS a la granja de servidores**

1.  Desde el servidor de Azure AD Connect, haga doble clic en el **Azure AD Connect** icono para iniciar el Asistente de Azure AD Connect.

2.  En la página de bienvenida, haga clic en **configurar**.

3.  En la página tareas adicionales, haga clic en **implementar un servidor de federación adicional** y, a continuación, haga clic en **siguiente**.

4.  En la página Conectar a Azure AD, escriba el nombre de usuario y la contraseña de una cuenta con permisos de administrador Global y, a continuación, haga clic en **siguiente**.

5.  En la página de credenciales de administrador de dominio, escriba el nombre de usuario y la contraseña de una cuenta con permisos de administrador de dominio y haga clic en **siguiente**.

6.  Haga clic en **examinar** y seleccione el archivo de certificado usará al configurar la granja de AD FS con Azure AD Connect.

7.  Haga clic en **escribir contraseña** para abrir el cuadro de diálogo de contraseña del certificado.

8.  Escriba la contraseña del certificado en el campo de contraseña y, a continuación, haga clic en **Aceptar**.

9.  Haz clic en **Siguiente**.

10. En la página de servidores de AD FS, escriba el nombre o la dirección IP del nuevo servidor de AD FS y haga clic en **agregar**.

11. En la página Listo para configurar, haga clic en **instalar**.

12. En la página Instalación finalizada, haga clic en **Exit**.

#### <a name="raising-the-adfs-farm-behavior-level"></a>Elevar el nivel de comportamiento de granja de servidores de AD FS

Al implementar un servidor de ADFs que supera el nivel actual del entorno, como tener un AD FS en Windows Server 2012 R2 y, a continuación, agregar que un servidor de Windows de AD FS 2016, el nivel de comportamiento de la granja de servidores debe aumentarse. Esto es necesario para asegurarse de que el entorno usa las funciones y la información más actualizada.

**Para elevar el nivel de comportamiento de granja de servidores de AD FS**

1.  Navegue hasta el AD FS de Windows Server 2016.

2.  Abra una sesión de PowerShell de administrador.

3.  Escriba el siguiente comando:  **\$cred = Get-Credential**

4.  Aparecerá una ventana que pide las credenciales, escriba las credenciales de administrador de dominio.

5.  A continuación, escriba este comando: **Invoke-AdfsFarmBehaviorLevelRaise -Credential \$cred**

6.  Aparecerá un mensaje que pregunta, **¿desea continuar con esta operación?** A continuación, escriba **un** para aceptar el símbolo del sistema.

7.  Cuando haya completado el comando, el nivel de comportamiento de la granja de servidores se programa de instalación y está listo.

#### <a name="enabling-mobile-device-extension-logging"></a>Habilitar el registro de la extensión de dispositivos móviles

La extensión de dispositivos móviles puede registrar las solicitudes que recibe de los dispositivos del usuario final. El registro está deshabilitado de forma predeterminada y se recomienda habilitar solo el registro en un escenario de solución de problemas. Todas las solicitudes, desde dispositivos móviles y equipos de escritorio, para arrancar o adquirir una licencia de uso final se registran en la base de datos de registro de AD RMS o la cuenta de almacenamiento de Azure. Registro MDE creará dos tablas adicionales para el servidor de SQL Server usados por AD RMS: el cliente Depurar tabla del registro y la tabla del registro de rendimiento de cliente.

**Para habilitar el registro de extensión para dispositivos móviles**

1.  En un servidor de AD RMS, abra Windows PowerShell como administrador.

2.  Escriba el siguiente comando y presione **ENTRAR**: **Import-Module AdRmsAdmin**

3.  Escriba el siguiente comando y presione **ENTRAR**: **New-PSDrive-nombre AdrmsCluster - PsProvider AdRmsAdmin-raíz https://localhost**

4.  Escriba el siguiente comando y presione **ENTRAR**: **Set-ItemProperty-Path AdrmsCluster:\\ -IsLoggingEnabled nombre-valor \$true**

Si usas registro MDE para solucionar problemas, se recomienda deshabilitarlo tras solucionar el problema.

**Para deshabilitar el registro de extensión para dispositivos móviles**

1.  En un servidor de AD RMS, abra Windows PowerShell como administrador.

2.  Escriba el siguiente comando y presione **ENTRAR**: **Import-Module AdRmsAdmin**

3.  Escriba el siguiente comando y presione **ENTRAR**: **New-PSDrive-nombre AdrmsCluster - PsProvider AdRmsAdmin-raíz https://localhost**

4.  Escriba el siguiente comando y presione **ENTRAR**: **Set-ItemProperty-Path AdrmsCluster:\\ -IsLoggingEnabled nombre-valor \$false**

### <a name="upgrading-ad-rms-to-windows-server-2016"></a>Actualización de AD RMS a Windows Server 2016

Las secciones siguientes proporcionan instrucciones sobre cómo agregar un servidor basado en Windows Server 2016 de AD RMS en el clúster actual de Windows Server 2012 R2. El servidor se agregará al clúster y la información se replicará a él para que el servidor de AD RMS anterior puede utilizarse para liberar recursos.

Después de agregar un servidor basado en Windows Server 2016 AD RMS se agregó a su clúster de AD RMS, todos los nodos basados en las versiones anteriores de Windows quedará inactivos. Tras ello puede desaprovisionar los servidores (por ejemplo, apagar, reasignar o volver a instalar con Windows Server 2016 para unirse al clúster de AD RMS). 

Puede implementar más servidores de AD RMS en el clúster para admitir la carga en la implementación de AD RMS. También puede realizar esta acción si se produce un aumento de tráfico a los servidores de AD RMS.

Esta guía no trata los pasos necesarios para modificar los mecanismos que puede que esté usando en su entorno para excluir los servidores que está en desuso y son los que va a agregar al clúster de equilibrio de carga. 

#### <a name="adding-a-2016-ad-rms-server"></a>Agregar un servidor de 2016 AD RMS

Si el clúster de AD RMS usa un módulo de seguridad de Hardware en lugar de una clave administrada centralmente para su certificado de emisor de licencias de servidor, deberá instalar el software y otros artefactos HSM (por ejemplo, los archivos de clave y extremos) en el servidor antes de instalar AD RMS. También necesitará conectar HSM en el servidor, físicamente o a través de las configuraciones de red relevantes. Siga la Guía de HSM para realizar estos pasos. 

**Para agregar un servidor de 2016 AD RMS**

1.  Instale el rol de AD RMS en la implementación de Windows Server 2016 deseada.

2.  Una vez finalizada la instalación, seleccione el vínculo para **realizar configuración adicional**.

3.  Seleccione **unirse a un clúster de AD RMS existente** y haga clic en **siguiente**.

4.  En el **Seleccionar base de datos de configuración** , escriba el registro CNAME especificado en el DNS para el servidor SQL 2016 (FQDN).

5.  Haga clic en **lista** en la segunda línea y seleccione el **DefaultInstance** en la lista desplegable.

6.  En **nombre de la base de datos de configuración**, seleccione el menú desplegable y elija la configuración de DRM que aparece. Haga clic en **Siguiente**.

7.  En el **información de la base de datos** , escriba la contraseña de clave de clúster en el campo proporcionado. A continuación, haga clic en **siguiente**.

8.  En la siguiente página del asistente, especifique la cuenta de servicio de AD RMS y proporcione la contraseña para ella y haga clic en **siguiente** una vez que se ha comprobado.

9.  Una vez el **sitio Web del clúster** simplemente aparece en la página, asegúrese de que se ha seleccionado el sitio web apropiado y haga clic en **siguiente**.

10. En el **elegir un certificado de autenticación de servidor** , seleccione el certificado importado de SSL y haga clic en **siguiente**.

11. Haga clic en **Instalar** para empezar la instalación.

12. Cuando finalice la configuración, deberá iniciar sesión en off y vuelva a abrirla para administrar AD RMS.

13. Una vez que se ha vuelto, abra **administrador del servidor** seleccione **herramientas** y, a continuación, **Active Directory Rights Management**. La ventana de administración debe aparecerá e indicará que el clúster tiene el servidor adicional en el clúster.

14. Si se instaló la extensión de dispositivos móviles de AD RMS en el clúster original de AD RMS, deberá instalar también la MDE en los nodos de clúster actualizado. Siga las instrucciones en la documentación de MDE agregar MDE al clúster de AD RMS. En este momento, puede reasignar todos los nodos preexistentes o actualizarlos a Windows Server 2016 y volver a unirse a ellos en el clúster de AD RMS mediante el mismo proceso descrito anteriormente. 

### <a name="configuring-windows-server-2016-web-application-proxy-wap"></a>Configuración de Proxy de aplicación de Windows Server 2016 Web (WAP)

Las secciones siguientes proporcionan instrucciones sobre las tareas operativas, es posible que deba realizar en la implementación de Proxy de aplicación Web. Este es un paso opcional, no es necesario si va a publicar AD RMS a Internet a través de otros mecanismos. 

#### <a name="adding-a-windows-server-2016-wap-server"></a>Agregar un servidor WAP de Windows Server 2016

Puede implementar servidores Proxy de aplicación Web adicionales para admitir la implementación de AD RMS. Puede realizar esta acción si se produce un aumento de tráfico a los servidores de AD RMS, o si necesita retirar uno de los servidores que se utiliza actualmente para el Proxy de aplicación Web.

**Para agregar un servidor Proxy de aplicación Web de 2016**

1.  Desde el servidor que quiere que el programa de instalación como un Proxy de aplicación Web, vaya a la consola de administrador del servidor y haga clic en **agregar roles y características**.

2.  En el **agregar Roles y características Asistente**, haga clic en **siguiente** hasta llegar a la pantalla de selección de rol de servidor.

3.  En la pantalla Seleccionar Roles de servidor, seleccione **acceso remoto**y, a continuación, haga clic en **siguiente** hasta que vuelva a la pantalla Seleccionar Roles de servidor.

4.  En la pantalla Seleccionar Roles de servidor, seleccione **Proxy de aplicación Web**, haga clic en **agregar características**y, a continuación, haga clic en **siguiente**.

5.  En la pantalla Confirmar selecciones de instalación, haga clic en **instalar**.

6.  Una vez completada la instalación, haga clic en **cerrar**.

7.  Ahora es momento de configurar el servidor. Para ello, abra la consola de administración de acceso remoto en el servidor Proxy de aplicación Web. Abra el **iniciar** menú, escriba **RAMgmtUI.exe**y, a continuación, seleccione la aplicación.

8.  En el panel de navegación, haga clic en **Proxy de aplicación Web**.

9.  En la consola de administración de acceso remoto, haga clic en **ejecutar el Asistente para configuración de Proxy de aplicación Web**. Una vez en el asistente, haga clic en **siguiente**.

10. En la pantalla de servidor de federación, escriba el nombre de dominio completo del servidor de AD FS (ex. ADFS.contoso.com) y, a continuación, escriba las credenciales para que un administrador en el servidor de AD FS.

11. En la pantalla de certificado de Proxy de AD FS, en la lista de certificados instalados actualmente en el servidor Proxy de aplicación Web, seleccione un certificado que se usará al Proxy de aplicación Web de proxy de AD FS y, a continuación, haga clic en **siguiente**.

12. En la pantalla de confirmación, revise la configuración, a continuación, haga clic en **configurar**.

13. Una vez completada la configuración, haga clic en **cerrar**.

#### <a name="dns-configuration-for-2016-wap-server"></a>Configuración de DNS para 2016 servidor WAP

Una vez que el servidor Proxy de aplicación Web de Windows Server 2016 se puso en su lugar, deberá realizar algunos cambios DNS. Esto requerirá el uso de un servicio, como GoDaddy para señalar el ADFS y servicios de AD RMS en el servidor WAP de 2016.

**Para señalar el DNS en el servidor WAP**

1.  Vaya al sitio Web de su proveedor (p. ej. GoDaddy).

2.  Vaya a administración de dominios y, a continuación, la administración de DNS.

3.  Busque el servicio AD FS y AD RMS y reemplace el **apunta a** parte con la dirección IP pública del servidor WAP 2016 y **guardar**.

4.  Los cambios pueden tardar en propagarse, pero cuando lo hagan este programa de instalación se completará.

#### <a name="enabling-debugging-logs"></a>Habilitar los registros de depuración

Información de registro detallada está disponible en los servidores Proxy de aplicación Web. Puede configurar el registro de depuración avanzado mediante el Visor de eventos. Opciones de configuración adicionales también pueden seleccionarse para el tamaño de los registros para ayudar a garantizar que el análisis son útiles para el Visor.

**Habilitar los registros de depuración para el Proxy de aplicación Web**

1.  Abra el **Visor de eventos** consola en el Proxy de aplicación Web.

2.  Expanda el **Microsoft** nodo.

3.  Expanda el **Windows** nodo.

4.  Abra el **Proxy de aplicación Web** registros.

5.  A continuación, podrá abrir el **Admin** registros.

6.  Abra el **acción** menú situado en la esquina superior izquierda y, a continuación, seleccione **propiedades**.

7.  En el **General** ficha, elija la opción de **Habilitar registro**.

8.  Por último, es posible personalizar el tamaño máximo del registro y lo que sucede cuando se alcanza el tamaño máximo de registro de eventos.

### <a name="configuring-high-availability-for-windows-server-2016-services"></a>Configuración de alta disponibilidad para servicios de Windows Server 2016

Las secciones siguientes proporcionan instrucciones sobre las tareas operativas, es posible que deba configurar el entorno de Windows Server 2016 de alta disponibilidad.

#### <a name="adding-a-2016-ad-rms-server-for-high-availability"></a>Agregar un servidor de 2016 AD RMS para la alta disponibilidad

Puede implementar servidores de AD RMS adicionales para configurar alta disponibilidad. Puede realizar esta acción si se produce un aumento de tráfico a los servidores de AD RMS.

**Para agregar un servidor de RMS AD 2016 de alta disponibilidad**

1.  Instale el rol de AD RMS en la implementación de Windows Server 2016 deseada.

2.  Una vez finalizada la instalación, seleccione el vínculo para **realizar configuración adicional**.

3.  Seleccione **unirse a un clúster de AD RMS existente** y haga clic en **siguiente**.

4.  En el **Seleccionar base de datos de configuración** , escriba el registro CNAME especificado en el DNS para el servidor SQL 2016 (FQDN).

5.  Haga clic en **lista** en la segunda línea y seleccione el **DefaultInstance** en la lista desplegable.

6.  En **nombre de la base de datos de configuración**, seleccione el menú desplegable y elija la configuración de DRM que aparece. Haga clic en **Siguiente**.

7.  En el **información de la base de datos** , escriba la contraseña de clave de clúster en el campo proporcionado. A continuación, haga clic en **siguiente**.

8.  En la siguiente página del asistente, especifique la cuenta de servicio de AD RMS y proporcione la contraseña para ella y haga clic en **siguiente** una vez que se ha comprobado.

9.  Una vez el **sitio Web del clúster** simplemente aparece en la página, asegúrese de que se ha seleccionado el sitio web apropiado y haga clic en **siguiente**.

10. En el **elegir un certificado de autenticación de servidor** , seleccione el certificado importado de SSL y haga clic en **siguiente**.

11. Haga clic en **Instalar** para empezar la instalación.

12. Cuando finalice la configuración, deberá iniciar sesión en off y vuelva a abrirla para administrar AD RMS.

13. Una vez que se ha vuelto, abra **administrador del servidor** seleccione **herramientas** y, a continuación, **Active Directory Rights Management**. La ventana de administración debe aparecerá e indicará que el clúster tiene el servidor adicional en el clúster.

14. Después de confirmar la instalación del servidor, configurar el servicio de equilibrio de carga para equilibrar la carga entre los diferentes servidores de AD RMS en el clúster.

#### <a name="adding-a-windows-server-2016-ad-fs-server-for-high-availability"></a>Adición de un servidor de Windows Server 2016 AD FS para alta disponibilidad

Puede implementar servidores de AD FS adicionales para configurar alta disponibilidad. Puede realizar esta acción si se produce un aumento de tráfico a los servidores de AD FS. **Nota: después de aumentar el nivel de comportamiento de la granja de servidores, una nueva entrada de la base de datos se escribirá en el SQL Server 2016 (Adfs Configv3) y la base de datos de configuración anterior debe eliminarse antes de continuar con estos pasos.**

**Para agregar el servidor de AD FS en Windows Server 2016 de alta disponibilidad**

1.  Instale el rol de AD RMS en la implementación de Windows Server 2016 deseada.

2.  Una vez finalizada la instalación, seleccione el vínculo para **configurar el servicio de federación en este servidor**.

3.  En la sección bienvenida del asistente, elija la opción de **agregar un servidor de federación a una granja de servidores de federación** y, a continuación, haga clic en **siguiente**.

4.  Especifique la cuenta de administrador apropiado y haga clic en **siguiente**.

5.  En el **especificar granja de servidores** página, elegir el **especificar ubicación de la base de datos para una granja existente mediante SQL Server** , a continuación, escriba el registro CNAME para el servicio SQL para el nombre de Host de la base de datos y haga clic en **siguiente**.

6.  En el **especificar cuenta de servicio** área del asistente, escriba las credenciales de la cuenta de servicio de AD FS y, a continuación, haga clic en **siguiente**.

7.  En **Revisar opciones**, haga clic en **siguiente**.

8.  Haga clic en **configurar** cuando esté disponible el botón.

9.  Después de la configuración, reinicie la máquina.

10. Después de confirmar la configuración del servidor, equilibrio de carga en los servidores de AD FS según sea necesario.

#### <a name="adding-a-windows-server-2016-wap-server-for-high-availability"></a>Agregar un servidor WAP de Windows Server 2016 de alta disponibilidad

Puede implementar servidores WAP adicionales para configurar alta disponibilidad. Puede realizar esta acción si se produce un aumento de tráfico a los servidores de AD RMS.

**Para agregar un servidor Proxy de aplicación Web de Windows Server 2016 de alta disponibilidad**

1.  Desde el servidor que quiere que el programa de instalación como un Proxy de aplicación Web, vaya a la consola de administrador del servidor y haga clic en **agregar roles y características**.

2.  En el **agregar Roles y características Asistente**, haga clic en **siguiente** hasta llegar a la pantalla de selección de rol de servidor.

3.  En la pantalla Seleccionar Roles de servidor, seleccione **acceso remoto**y, a continuación, haga clic en **siguiente** hasta que vuelva a la pantalla Seleccionar Roles de servidor.

4.  En la pantalla Seleccionar Roles de servidor, seleccione **Proxy de aplicación Web**, haga clic en **agregar características**y, a continuación, haga clic en **siguiente**.

5.  En la pantalla Confirmar selecciones de instalación, haga clic en **instalar**.

6.  Una vez completada la instalación, haga clic en **cerrar**.

7.  Ahora es momento de configurar el servidor. Para ello, abra la consola de administración de acceso remoto en el servidor Proxy de aplicación Web. Abra el **iniciar** menú, escriba **RAMgmtUI.exe**y, a continuación, seleccione la aplicación.

8.  En el panel de navegación, haga clic en **Proxy de aplicación Web**.

9.  En la consola de administración de acceso remoto, haga clic en **ejecutar el Asistente para configuración de Proxy de aplicación Web**. Una vez en el asistente, haga clic en **siguiente**.

10. En la pantalla de servidor de federación, escriba el nombre de dominio completo del servidor de AD FS (ex. ADFS.contoso.com) y, a continuación, escriba las credenciales para que un administrador en el servidor de AD FS.

11. En la pantalla de certificado de Proxy de AD FS, en la lista de certificados instalados actualmente en el servidor Proxy de aplicación Web, seleccione un certificado que se usará al Proxy de aplicación Web de proxy de AD FS y, a continuación, haga clic en **siguiente**.

12. En la pantalla de confirmación, revise la configuración, a continuación, haga clic en **configurar**.

13. Una vez completada la configuración, haga clic en **cerrar**.

14. Después de confirmar la instalación del servidor, equilibrio de carga entre los servidores WAP en la red Perimetral.

#### <a name="adding-a-sql-server-2016-node-for-always-on-high-availability"></a>Agregar un nodo de SQL Server 2016 de alta disponibilidad de AlwaysOn

Puede implementar servidores SQL adicionales para configurar alta disponibilidad de AlwaysOn. Puede realizar esta acción si se produce un aumento de tráfico a los servidores de AD RMS. **Nota: asegúrese de que ambos servidores SQL Server tiene el puerto de entrada abierto 5022.**

**Para agregar un servidor SQL server 2016 para alta disponibilidad de AlwaysOn**

1.  Desde el servidor que quiere que el programa de instalación como un servidor de SQL Server 2016 adicional, vaya a la consola de administrador del servidor y haga clic en **agregar roles y características**.

2.  Haga clic en **siguiente** hasta que el **seleccionar características** cuadro de diálogo.

3.  Seleccione el **agrupación en clústeres de conmutación por error** casilla de verificación. **Nota: siga este paso para el SQL server 2016 server original también para que ambos servidores SQL Server tiene la característica de clústeres de conmutación por error.**

4.  Haga clic en **instalar** para instalar la característica de agrupación en clústeres de conmutación por error.

5.  Ahora, abra **administrador del servidor** y seleccione **herramientas** , a continuación, **Administrador de clústeres de conmutación por error**.

6.  En el panel de menú de la izquierda, haga clic en **Administrador de clústeres de conmutación por error** y seleccione **crear clúster**

7.  Se abrirá el **Asistente para crear clúster**.

8.  Los servidores de SQL server 2016 que se usará para la alta disponibilidad de AlwaysOn y escribirlos en, a continuación, haga clic en Buscar **siguiente**.

9.  Recibirá una advertencia de validación. Seleccione **Sí** para validar los nodos del clúster y, a continuación, haga clic en **siguiente**.

10. En el **opciones de pruebas** página, seleccione la opción **ejecutar todas las pruebas** y haga clic en **siguiente.**

11. **Nota: El Asistente para la validación de clúster debe devolver varios mensajes de advertencia, especialmente si va a usar no almacenamiento compartido. Aparte de eso, si encuentra algún mensaje de error deberá corregirlos antes de crear el clúster de conmutación por error de Windows Server**.

12. En el **punto de acceso para administrar el clúster** cuadro de diálogo, escriba el nombre del clúster y la dirección IP virtual para el clúster de conmutación por error de Windows Server, a continuación, haga clic en **siguiente**.

13. Compruebe que la configuración es correcta en **resumen** y haga clic en **finalizar**.

14. En el **Administrador de clústeres de conmutación por error,** haga doble clic en el clúster y seleccione **más acciones** , a continuación, elija **configurar opciones de quórum de clúster**

15. Haga clic en **siguiente** y, a continuación, elija la opción para **selección del testigo de quórum** y posicionamiento **siguiente** nuevo.

16. En el **seleccionar testigo de quórum** , seleccione el **configurar un testigo de recurso compartido de archivos** opción. Haga clic en **Siguiente**.

17. Seleccione **examinar** y busque la ruta de acceso del recurso compartido de archivos que desea usar en el cuadro de diálogo de la ruta de acceso de recurso compartido de archivo. Haz clic en **Siguiente**.

18. En la página de confirmación, haga clic en **siguiente**.

19. En la página Resumen, haga clic en **finalizar**.

20. Ahora, abra el **iniciar** menú y buscar **Administrador de configuración de SQL Server**.

21. Haga clic en el nombre de SQL Server y seleccione **propiedades**.

22. En el cuadro de diálogo Propiedades, seleccione la **alta disponibilidad de AlwaysOn** ficha. Compruebe el **habilitar los grupos de disponibilidad AlwaysOn** casilla de verificación. Haga clic en **Aceptar**. **Nota: hacer esto en SQL server 2016 servidores.**

23. A continuación, reinicie el servicio SQL Server.

24. Ahora, abra el **iniciar** menú y buscar **SQL Server Management Studio** y en el panel de navegación izquierdo, haga clic en **grupos de disponibilidad** y haga clic en **Asistente para nuevo grupo de disponibilidad** , a continuación, haga clic en **siguiente**.

25. En el **especificar el nombre de grupo de disponibilidad** página Elegir un nombre de grupo (Ex.SQLAvailabilityGroup2016). Haga clic en **Siguiente**.

26. En el **seleccionar bases de datos** , especifique las bases de datos. A continuación, haga clic en Siguiente. **Nota: algunas bases de datos que deba ser copia de seguridad nuevo o poner en modo de recuperación completa**.

27. Una vez activado el **especificar réplicas** página, haga clic en el **Agregar réplica** botón y seleccione el otro servidor de SQL 2016.

28. Después de agregar otro servidor, haga clic en las casillas de verificación y establezca el servidor secundario a una réplica secundaria legible.

29. Vaya a la **extremos** ficha y haga clic en el **actualizar** opción. Mientras también aquí, desplazarse y asegúrese de que es la misma cuenta de servicio en el nodo principal y secundario.

30. Ahora, elija el **preferencias de copia de seguridad** pestaña y seleccione el **preferir secundaria** opción.

31. Ir a la **escucha** ficha.

32. Especifique un nombre (p. ej. SQLListener) y asegúrese de que el puerto es **1433** y, a continuación, haga clic en **siguiente**.

33. En el **seleccionar sincronización de datos inicial** página del asistente, elija la **completa** opción y especifique la ubicación de red accesible por todos los servidores SQL y, a continuación, haga clic en **siguiente**.

34. Por último, haga clic en **finalizar** y se completará el proceso.

### <a name="decommission-windows-server-2012-r2-nodes"></a>Retirar nodos de Windows Server 2012 R2

Las secciones siguientes proporcionan instrucciones sobre las tareas operativas, es posible que deba quitar los servidores de Windows Server 2012 R2 después de actualizar correctamente el clúster de AD RMS para Windows Server 2016.

#### <a name="removing-a-windows-server-2012-r2-ad-rms-server"></a>Quitar un servidor Windows Server 2012 R2 AD RMS

Puede quitar servidores de AD RMS innecesarios tras una actualización. Puede realizar esta acción cuando se pasa a ser necesarios para retirar los servidores de AD RMS.

**Para quitar un** **server de Windows Server 2012 R2 AD RMS**

1.  En el servidor de Windows Server 2012 R2 AD RMS en el administrador del servidor, seleccione **administrar** desde la parte superior derecha de los menús y, a continuación, elija **quitar Roles y características**.

2.  El **quitar Roles y características Asistente** abrirá copias de seguridad y en el **antes de comenzar** pantalla, haga clic en **siguiente**.

3.  En el **selección de servidor** pantalla, haga clic en **siguiente**.

4.  En el **Roles de servidor** pantalla, quitar la marca de verificación junto a **Active Directory Rights Management Services** y haga clic en **siguiente**.

5.  En el **características** pantalla, haga clic en **siguiente**.

6.  En el **confirmación** pantalla, haga clic en **quitar**.

7.  Una vez que se complete, reinicie el servidor.

8.  Ahora puede cerrar este servidor y reasignar los recursos según sea necesario.

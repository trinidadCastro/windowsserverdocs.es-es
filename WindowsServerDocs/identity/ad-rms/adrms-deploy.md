---
ms.assetid: e6fa9069-ec9c-4615-b266-957194b49e11
title: Actualizar AD RMS a Windows Server 2016
author: msmbaldwin
ms.author: esaggese
ms.date: 05/30/2019
ms.prod: windows-server
ms.topic: article
ms.openlocfilehash: 88af85f8e670b9c23b503e0f79af2ce8f10d045e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854858"
---
# <a name="upgrading-ad-rms-to-windows-server-2016"></a>Actualizar AD RMS a Windows Server 2016

## <a name="introduction"></a>Introducción

Active Directory Rights Management Services (AD RMS) es un servicio de Microsoft que protege documentos y correos electrónicos confidenciales. A diferencia de los métodos de protección tradicionales, como firewalls y ACL, AD RMS el cifrado y la protección son persistentes independientemente de la ubicación de un archivo o de su transporte. 

En este documento se proporcionan instrucciones para migrar desde Windows Server 2012 R2 con SQL Server 2012 a Windows Server 2016 y SQL Server 2016. Se puede usar el mismo proceso para migrar desde versiones anteriores pero admitidas de AD RMS.
Tenga en cuenta que Active Directory Rights Management Services ya no está en desarrollo activo y, para las funcionalidades más recientes, los clientes deben considerar la posibilidad de migrar a [Azure Information Protection](https://azure.microsoft.com/services/information-protection/), que ofrece un conjunto de características mucho más completo con compatibilidad con dispositivos y aplicaciones más completas. 

Para obtener información sobre cómo migrar a Azure Information Protection desde AD RMS sin tener que volver a proteger el contenido, consulte [la documentación de migración de Azure Information Protection](https://docs.microsoft.com/azure/information-protection/migrate-from-ad-rms-to-azure-rms).

## <a name="about-the-environment-used-in-this-guide"></a>Acerca del entorno usado en esta guía

AD FS es un componente opcional de una instalación de AD RMS. En esta guía, se supone el uso de ADFS. Si ADFS no se ha usado en su entorno para admitir AD RMS usuarios, puede omitir todos los pasos que hacen referencia a ADFS.

En esta guía, SQL Server se actualiza a SQL Server 2016 realizando una instalación paralela y moviendo las bases de datos a través de una copia de seguridad. Como alternativa, si puede actualizar los servidores de AD RMS y de base de datos de ADFS a SQL Server 2016 en contexto, puede pasar a la siguiente sección de este documento después de haber hecho esto sin tener que seguir los pasos de esta sección.  

## <a name="installation"></a>Installation

### <a name="configuring-sql-server-2016"></a>Configuración de SQL Server 2016

En la siguiente sección se detallan las tareas de implementación relacionadas directamente con la configuración de SQL Server 2016. Esta guía se centra en el uso del Administrador del servidor y el SQL Server Management Studio para completar estas tareas.

Estos pasos se deben completar en una instalación SQL Server 2016. Instale SQL Server 2016 en el hardware adecuado según las prácticas y directivas estándar de su organización. 

#### <a name="preparing-the-sql-server"></a>Preparación del SQL Server

En la siguiente sección se detalla cómo preparar el SQL Server para que se pueda actualizar a SQL Server 2016 antes de actualizar otros servicios de la plataforma de AD RMS para usar Windows Server 2016.

##### <a name="adding-cname-for-sql-server-2016-to-dns"></a>Agregando CNAME para SQL Server 2016 a DNS

El CNAME se usa para asegurarse de que el programa de instalación de Windows Server 2016 va a obtener los datos adecuados, ya que se señalará al nuevo SQL Server 2016. **Nota: Si ya usa un CNAME para el servicio ADFS y AD RMS, puede pasar a los pasos siguientes.**


**Para agregar un CNAME para SQL Server 2016 a DNS**

1.  Inicie sesión en el controlador de dominio de Windows Server 2012 R2 con credenciales de administrador de dominio.

2.  Abra el Administrador del servidor.

3.  Haga clic en **herramientas** y seleccione **DNS** para abrir el administrador de DNS.

4.  En el panel de navegación izquierdo, expanda el controlador de dominio y abra las **zonas de búsqueda directa**.

5.  Abra los recursos de dominio adecuados y, a continuación, haga clic con el botón derecho en el panel de vista derecho y seleccione **nuevo alias (CNAME)** para empezar a crear el CNAME.

6.  En nombre de alias, escriba un nombre lógico para diferenciarlo de otro que puede estar presente (por ejemplo, SQLADRMS o SQLADFS)

7.  Después de escribir el nombre, proporcione el FQDN para el host de destino que será el nuevo servidor SQL Server 2016. antiguo. SQL2016.contoso.com)

8.  Una vez que se haya especificado toda la información, haga clic en **Aceptar**.

##### <a name="backup-the-ad-rms-and-adfs-databases"></a>Copia de seguridad de las bases de datos de AD RMS y ADFS

Las bases de datos de AD RMS y ADFS contienen información crítica necesaria para AD RMS, como la clave pública del certificado de emisor de licencias de servidor, las plantillas de directiva de permisos, los datos de configuración de ADFS e información de registro. Sin estas bases de datos, los clientes no pueden emitir licencias para consumir contenido protegido, entre otros problemas.

De las bases de datos, la base de datos de configuración de AD RMS se considera lo más importante, ya que almacena el SLC, las plantillas de directiva de permisos, las claves de los usuarios y la información de configuración. Por lo tanto, aunque debe tener cuidado para realizar una copia de seguridad de todas las bases de datos de AD RMS y ADFS, debe planear la copia de seguridad de la base de datos de configuración con regularidad.

La base de datos de registro almacena información acerca de las solicitudes de usuario al clúster de AD RMS para certificados y licencias de uso. La estrategia de copia de seguridad de esta base de datos debe basarse en la Directiva de la empresa para conservar este tipo de información.

La base de datos de servicios de directorio no es crítica para AD RMS funcionalidad y, si se pierden los datos más recientes, la base de datos se volverá a rellenar con información, ya que el servidor de AD RMS recibe solicitudes de certificados y licencias de uso. No es necesario realizar una copia de seguridad de esta base de datos con regularidad, pero es necesario tener al menos una copia de la base de datos tal y como se configuró originalmente después de implementar AD RMS.

**Para hacer una copia de seguridad de una base de datos de AD RMS o ADFS con Microsoft SQL Server**

1.  Inicie sesión en el servidor de base de datos de AD RMS de Windows Server 2012 R2 con SQL 2012.

2.  Haga clic en **Inicio**, en **todos los programas**, en **Microsoft SQL Server**y, a continuación, en **SQL Server Management Studio**.

3.  En la ventana **conectar con el servidor** , confirme que el servidor que hospeda las bases de datos de AD RMS está en el cuadro **nombre del servidor** y haga clic en **conectar**.

4.  Expanda **bases de datos**. Haga clic con el botón derecho en la base de datos adecuada (**DRM** y **ADFS**), seleccione **tareas**y haga clic en **copia de seguridad**.

5.  Repita el paso 4 para las bases de datos restantes.

6.  Asegúrese de que otras máquinas de la red pueden tener acceso a la copia de seguridad de las bases de datos o mediante un dispositivo de almacenamiento, ya que se necesitarán para pasos posteriores durante la migración.

Ahora puede almacenar las copias de la base de datos en una ubicación segura. Recuerde realizar copias de seguridad de las bases de datos con frecuencia.

##### <a name="adding-domain-admin-sql-ad-rms-andor-adfs-service-account-to-sql-server-2016"></a>Agregar la cuenta de servicio de administrador de dominio, SQL, AD RMS o AD FS a SQL Server 2016

En los pasos siguientes se muestra cómo agregar las distintas cuentas de servicio a SQL Server 2016 para ayudarle a migrar los datos del entorno de Windows Server 2012 R2. Esto proporcionará los permisos adecuados al intentar tener acceso al contenido y controlar los datos.

**Para agregar el administrador de dominio, SQL, AD RMS o la cuenta de servicio ADFS a SQL Server**

1.  Inicie sesión en el servidor con SQL Server 2016 como cuenta de administrador local.

2.  Haga clic en **Inicio**, en **todos los programas**, en **Microsoft SQL Server**y, a continuación, en **SQL Server Management Studio**.

3.  En la ventana **conectar con el servidor** , confirme que el servidor que hospeda las bases de datos de AD RMS está en el cuadro **nombre del servidor** y, para autenticación, haga clic en el menú desplegable y seleccione autenticación de **SQL Server**.

4.  En el campo **Inicio de sesión** , escriba el nombre de la cuenta de administrador local (por ejemplo, localadmin) y, a continuación, proporcione la contraseña adecuada y haga clic en **conectar**.

5.  Expanda **seguridad** y, a continuación, haga clic con el botón secundario en **inicios de sesión** y seleccione **nuevo inicio de sesión** en el menú contextual que aparece.

6.  Una vez que aparezca la ventana, escriba en la cuenta de administrador de dominio en el campo **nombre de inicio de sesión** (por ejemplo, Contoso\\ContosoAdmin)

7.  En el panel de navegación izquierdo, elija **roles de servidor**.

8.  A continuación, marque la casilla de **sysadmin** en los roles de servidor y haga clic en **Aceptar**.

9.  Reinicie la **Administración de SQL Server**.

10. En la ventana **conectar con el servidor** , confirme que el servidor que hospeda las bases de datos de AD RMS está en el cuadro **nombre del servidor** y, para autenticación, haga clic en el menú desplegable y seleccione **autenticación de Windows** y haga clic en **conectar**.

##### <a name="restoring-the-ad-rms-and-adfs-databases-to-sql-server-2016"></a>Restauración de las bases de datos de AD RMS y ADFS en SQL Server 2016

En los pasos siguientes se muestra cómo restaurar los datos de la instancia de SQL Server anterior a la nueva instancia de 2016. Esto permitirá que el nuevo SQL use los datos de configuración relevantes de las bases de datos de AD RMS y ADFS anteriores.

**Para restaurar los datos del SQL Server anterior al nuevo SQL Server**

1.  Inicie sesión en el servidor con SQL Server 2016 con la cuenta adecuada.

2.  En el panel de navegación izquierdo, haga clic con el botón derecho en **bases** de datos y seleccione **restaurar base de datos** para iniciar el proceso de restauración.

3.  En **origen** , seleccione **dispositivo** y, a continuación, busque la ubicación donde se almacenaron los archivos de base de datos en los pasos anteriores.

4.  Una vez seleccionados los archivos, haga clic en **Aceptar**.

5.  Asegúrese de que se han agregado todos los archivos de base de datos y complete el proceso haciendo clic en **Aceptar**.

### <a name="configuring-windows-server-2016-active-directory-federation-services-ad-fs"></a>Configuración de Servicios de federación de Active Directory (AD FS) de Windows Server 2016 (AD FS)

AD FS se ha implementado para proporcionar acceso de inicio de sesión único (SSO) a AD RMS como una aplicación. También se ha configurado con el AD RMS extensión de dispositivo móvil (MDE), que habilita la compatibilidad con Mac y dispositivos móviles para los usuarios finales.

En las secciones siguientes se proporcionan instrucciones sobre las tareas operativas que es posible que deba realizar en la implementación de AD FS.

#### <a name="adding-a-2016-ad-fs-server-to-the-farm"></a>Adición de un servidor 2016 AD FS a la granja de servidores

Puede implementar servidores de AD FS adicionales para admitir la implementación de AD RMS. Puede optar por realizar esta acción en caso de que se aumente el tráfico a los servidores de AD RMS, o aplicaciones adicionales, o bien si necesita retirar uno de los servidores que se usan actualmente para AD FS.

**Para agregar el servidor 2016 AD FS a la granja de servidores**

1.  En el servidor de Azure AD Connect, haga doble clic en el icono de **Azure ad Connect** para iniciar el asistente para Azure ad Connect.

2.  En la página de bienvenida, haga clic en **configurar**.

3.  En la página tareas adicionales, haga clic en **implementar un servidor de Federación adicional** y, a continuación, haga clic en **siguiente**.

4.  En la página conectar a Azure AD, escriba el nombre de usuario y la contraseña de una cuenta con permisos administrativos globales y, a continuación, haga clic en **siguiente**.

5.  En la página credenciales de administrador de dominio, escriba el nombre de usuario y la contraseña de una cuenta con permisos de administrador de dominio y haga clic en **siguiente**.

6.  Haga clic en **examinar** y seleccione el archivo de certificado que se usa al configurar la granja de AD FS mediante el Azure ad Connect.

7.  Haga clic en **escribir contraseña** para abrir el cuadro de diálogo contraseña de certificado.

8.  Escriba la contraseña del certificado en el campo contraseña y haga clic en **Aceptar**.

9.  Haga clic en **Siguiente**.

10. En la página servidores de AD FS, escriba el nombre o la dirección IP del nuevo servidor de AD FS y haga clic en **Agregar**.

11. En la página listo para configurar, haga clic en **instalar**.

12. En la Página Instalación completada, haga clic en **salir**.

#### <a name="raising-the-adfs-farm-behavior-level"></a>Elevar el nivel de comportamiento de la granja de AD FS

Cuando se implementa un servidor de ADFs que supera el nivel de entorno actual, como, que tiene un ADFS en Windows Server 2012 R2 y, a continuación, agrega un ADFS Windows Server 2016, es necesario aumentar el nivel de comportamiento de la granja. Esto es necesario para asegurarse de que el entorno esté usando la información y las funciones más actualizadas.

**Para elevar el nivel de comportamiento de la granja de AD FS**

1.  Vaya a Windows Server 2016 ADFS.

2.  Abra una sesión de PowerShell de administración.

3.  Escriba el siguiente comando: **\$CRED = Get-Credential**

4.  Aparecerá una ventana en la que se solicitan las credenciales, escriba en las credenciales de administrador de dominio.

5.  A continuación, escriba este comando: **Invoke-AdfsFarmBehaviorLevelRaise-Credential \$CRED**

6.  Aparecerá un mensaje en el que se le pregunta si **desea continuar con esta operación** . A continuación, escriba **un** para aceptar el aviso.

7.  Una vez completado el comando, el nivel de comportamiento de la granja de servidores estará configurado y listo.

#### <a name="enabling-mobile-device-extension-logging"></a>Habilitación del registro de extensiones de dispositivos móviles

La extensión de dispositivo móvil puede registrar las solicitudes que recibe de los dispositivos de usuario final. El registro está deshabilitado de forma predeterminada y solo se recomienda habilitar el registro en un escenario de solución de problemas. Todas las solicitudes, desde dispositivos móviles y equipos de escritorio, hasta el arranque o la adquisición de una licencia de uso final se registran en la AD RMS base de datos de registro o en la cuenta de almacenamiento de Azure. El registro MDE creará dos tablas adicionales en el SQL Server que utiliza AD RMS: la tabla de registro de depuración de cliente y la tabla de registro de rendimiento de cliente.

**Para habilitar el registro de extensiones de dispositivos móviles**

1.  En un servidor de AD RMS, abra Windows PowerShell como administrador.

2.  Escriba el siguiente comando y presione **entrar**: **Import-Module AdRmsAdmin**

3.  Escriba el siguiente comando y presione **entrar**: **New-PSDrive-Name AdrmsCluster-PsProvider AdRmsAdmin-root https://localhost**

4.  Escriba el siguiente comando y presione **entrar**: **set-ItemProperty-Path AdrmsCluster:\\-Name IsLoggingEnabled-Value \$true**

Si usa el registro MDE para la solución de problemas, se recomienda deshabilitarlo después de solucionar el problema.

**Para deshabilitar el registro de extensiones de dispositivos móviles**

1.  En un servidor de AD RMS, abra Windows PowerShell como administrador.

2.  Escriba el siguiente comando y presione **entrar**: **Import-Module AdRmsAdmin**

3.  Escriba el siguiente comando y presione **entrar**: **New-PSDrive-Name AdrmsCluster-PsProvider AdRmsAdmin-root https://localhost**

4.  Escriba el siguiente comando y presione **entrar**: **set-ItemProperty-Path AdrmsCluster:\\-Name IsLoggingEnabled-Value \$false**

### <a name="upgrading-ad-rms-to-windows-server-2016"></a>Actualizar AD RMS a Windows Server 2016

En las secciones siguientes se proporcionan instrucciones sobre cómo agregar un servidor de AD RMS basado en Windows Server 2016 en el clúster de Windows Server 2012 R2 actual. El servidor se agregará al clúster y la información se replicará en él para que el servidor de AD RMS anterior pueda dejar de usarse para liberar recursos.

Después de agregar un servidor de AD RMS basado en Windows Server 2016 a su clúster de AD RMS, todos los nodos basados en versiones anteriores de Windows pasarán a estar inactivos. Una vez hecho esto, puede desaprovisionar los servidores (por ejemplo, cerrar, reasignar o reinstalar con Windows Server 2016 para unirse al clúster de AD RMS). 

Puede implementar servidores de AD RMS adicionales en el clúster para admitir la carga en la implementación de AD RMS. También puede optar por realizar esta acción en caso de que se aumente el tráfico a los servidores de AD RMS.

En esta guía no se tratan los pasos necesarios para modificar los mecanismos de equilibrio de carga que puede usar en su entorno para excluir los servidores que está desusada e incluir los que está agregando al clúster. 

#### <a name="adding-a-2016-ad-rms-server"></a>Adición de un servidor 2016 AD RMS

Si el clúster de AD RMS usa un módulo de seguridad de hardware en lugar de una clave administrada centralmente para su certificado de emisor de licencias de servidor, deberá instalar el software y otros artefactos de HSM (por ejemplo, archivos de clave y extremos) en el servidor antes de instalar AD RMS. También necesitará conectar el HSM al servidor, ya sea físicamente o a través de las configuraciones de red pertinentes. Siga las instrucciones de HSM para estos pasos. 

**Para agregar un servidor 2016 AD RMS**

1.  Instale el rol de AD RMS en la implementación de Windows Server 2016 deseada.

2.  Una vez finalizada la instalación, seleccione el vínculo para **realizar la configuración adicional**.

3.  Seleccione **unir un clúster de AD RMS existente** y haga clic en **siguiente**.

4.  En la página **Seleccionar base de datos de configuración** , escriba el CNAME especificado en el DNS para el servidor SQL Server 2016 (FQDN).

5.  Haga clic en **lista** en la segunda línea y seleccione el **DefaultInstance** en el menú desplegable.

6.  En **nombre**de la base de datos de configuración, seleccione el menú desplegable y elija la configuración de DRM que aparece. A continuación, haga clic en **Siguiente**.

7.  En la página **información de base de datos** , escriba la contraseña de la clave de clúster en el campo proporcionado. Después, haga clic en **siguiente**.

8.  En la página siguiente del asistente, especifique la cuenta de servicio de AD RMS y proporcione la contraseña correspondiente y haga clic en **siguiente** cuando se haya comprobado.

9.  Una vez que aparezca la página **sitio web del clúster** , solo tiene que asegurarse de que se ha seleccionado el sitio web adecuado y hacer clic en **siguiente**.

10. En la página **elegir un certificado de autenticación de servidor** , seleccione el certificado SSL importado y haga clic en **siguiente**.

11. Haga clic en **Instalar** para empezar la instalación.

12. Una vez finalizada la configuración, tendrá que cerrar la sesión y volver a iniciarla para administrar AD RMS.

13. Una vez que vuelva a iniciar sesión, Abra **Administrador del servidor** seleccione **herramientas** y, a continuación, **Active Directory Rights Management**. La ventana de administración debe aparecer e indicar que el clúster tiene el servidor adicional en el clúster.

14. Si el AD RMS extensión de dispositivo móvil se instaló en el clúster de AD RMS original, también debe instalar el MDE en los nodos de clúster actualizados. Siga las instrucciones de la documentación de MDE para agregar un MDE a su clúster de AD RMS. En este momento, puede reasignar todos los nodos preexistentes o actualizarlos a Windows Server 2016 y volver a unirlos al clúster de AD RMS con el mismo proceso descrito anteriormente. 

### <a name="configuring-windows-server-2016-web-application-proxy-wap"></a>Configurar el proxy de aplicación web (WAP) de Windows Server 2016

En las secciones siguientes se proporcionan instrucciones sobre las tareas operativas que es posible que deba realizar en la implementación del proxy de aplicación Web. Este es un paso opcional, no es necesario si va a publicar AD RMS a Internet a través de otros mecanismos. 

#### <a name="adding-a-windows-server-2016-wap-server"></a>Agregar un servidor WAP de Windows Server 2016

Puede implementar servidores proxy de aplicación web adicionales para admitir la implementación de AD RMS. Puede optar por realizar esta acción en caso de aumentar el tráfico a los servidores de AD RMS o si necesita retirar uno de los servidores que se usan actualmente para el proxy de aplicación Web.

**Para agregar un servidor proxy de aplicación web 2016**

1.  En el servidor que desea configurar como proxy de aplicación Web, vaya a la consola de Administrador del servidor y haga clic en **Agregar roles y características**.

2.  En el **Asistente para agregar roles y características**, haga clic en **siguiente** hasta llegar a la pantalla de selección de rol de servidor.

3.  En la pantalla Seleccionar roles de servidor, seleccione **acceso remoto**y, a continuación, haga clic en **siguiente** hasta que vuelva a la pantalla Seleccionar roles de servidor.

4.  En la pantalla Seleccionar roles de servidor, seleccione **proxy de aplicación web**, haga clic en **Agregar características**y, a continuación, haga clic en **siguiente**.

5.  En la pantalla confirmar selecciones de instalación, haga clic en **instalar**.

6.  Una vez finalizada la instalación, haga clic en **cerrar**.

7.  Ahora es el momento de configurar el servidor. Para ello, abra la consola de administración de acceso remoto en el servidor proxy de aplicación Web. Abra el menú **Inicio** , escriba **RAMgmtUI. exe**y, a continuación, seleccione la aplicación.

8.  En el panel de navegación, haz clic en **Proxy de aplicación web**.

9.  En la consola de administración de acceso remoto, haga clic en **ejecutar el Asistente para configuración de proxy de aplicación web**. Una vez en el asistente, haga clic en **siguiente**.

10. En la pantalla servidor de Federación, escriba el nombre de dominio completo del servidor de AD FS (por ejemplo, adfs.contoso.com) y, a continuación, escriba las credenciales de un administrador en el servidor de AD FS.

11. En la pantalla certificado de proxy de AD FS, en la lista de certificados instalados actualmente en el servidor proxy de aplicación Web, seleccione el certificado que usará el proxy de aplicación web para AD FS proxy y, a continuación, haga clic en **siguiente**.

12. En la pantalla confirmación, revise la configuración y haga clic en **configurar**.

13. Una vez completada la configuración, haga clic en **cerrar**.

#### <a name="dns-configuration-for-2016-wap-server"></a>Configuración de DNS para el servidor WAP 2016

Una vez que se haya colocado el servidor de proxy de aplicación Web de Windows Server 2016, se deberán realizar algunos cambios de DNS. Esto requerirá el uso de un servicio como GoDaddy para apuntar a ADFS y AD RMS Services en el servidor WAP 2016.

**Para señalar el DNS en el servidor WAP**

1.  Vaya al sitio web del proveedor (por ejemplo, GoDaddy).

2.  Vaya a administración de dominios y, después, a administración de DNS.

3.  Busque el servicio ADFS y el AD RMS y reemplace el **punto** por la dirección IP pública del servidor WAP 2016 y **guárdelo**.

4.  Los cambios pueden tardar tiempo en propagarse, pero una vez que lo hagan, se completará la instalación.

#### <a name="enabling-debugging-logs"></a>Habilitar los registros de depuración

Hay información de registro detallada disponible en los servidores proxy de aplicación Web. Puede configurar el registro de depuración avanzada mediante el Visor de eventos. También se puede seleccionar una configuración adicional para el tamaño de los registros con el fin de garantizar que el análisis sea útil para el visor.

**Habilitación de registros de depuración para el proxy de aplicación Web**

1.  Abra la consola de **visor de eventos** en el proxy de aplicación Web.

2.  Expanda el nodo **Microsoft** .

3.  Expanda el nodo **Windows** .

4.  Abra los registros del **proxy de aplicación web** .

5.  Podrá abrir los registros de **Administración** .

6.  Abra el menú **acción** , situado en la parte superior izquierda, y seleccione **propiedades**.

7.  En la pestaña **General** , elija la opción para **Habilitar el registro**.

8.  Por último, puede personalizar el tamaño máximo del registro y lo que sucede cuando se alcanza el tamaño máximo del registro de eventos.

### <a name="configuring-high-availability-for-windows-server-2016-services"></a>Configuración de alta disponibilidad para los servicios de Windows Server 2016

En las secciones siguientes se proporcionan instrucciones sobre las tareas operativas que puede necesitar para configurar el entorno de Windows Server 2016 en alta disponibilidad.

#### <a name="adding-a-2016-ad-rms-server-for-high-availability"></a>Adición de un servidor 2016 AD RMS para alta disponibilidad

Puede implementar más servidores de AD RMS para configurar la alta disponibilidad. Puede optar por realizar esta acción en caso de que se aumente el tráfico a los servidores de AD RMS.

**Para agregar un servidor 2016 AD RMS para alta disponibilidad**

1.  Instale el rol de AD RMS en la implementación de Windows Server 2016 deseada.

2.  Una vez finalizada la instalación, seleccione el vínculo para **realizar la configuración adicional**.

3.  Seleccione **unir un clúster de AD RMS existente** y haga clic en **siguiente**.

4.  En la página **Seleccionar base de datos de configuración** , escriba el CNAME especificado en el DNS para el servidor SQL Server 2016 (FQDN).

5.  Haga clic en **lista** en la segunda línea y seleccione el **DefaultInstance** en el menú desplegable.

6.  En **nombre**de la base de datos de configuración, seleccione el menú desplegable y elija la configuración de DRM que aparece. A continuación, haga clic en **Siguiente**.

7.  En la página **información de base de datos** , escriba la contraseña de la clave de clúster en el campo proporcionado. Después, haga clic en **siguiente**.

8.  En la página siguiente del asistente, especifique la cuenta de servicio de AD RMS y proporcione la contraseña correspondiente y haga clic en **siguiente** cuando se haya comprobado.

9.  Una vez que aparezca la página **sitio web del clúster** , solo tiene que asegurarse de que se ha seleccionado el sitio web adecuado y hacer clic en **siguiente**.

10. En la página **elegir un certificado de autenticación de servidor** , seleccione el certificado SSL importado y haga clic en **siguiente**.

11. Haga clic en **Instalar** para empezar la instalación.

12. Una vez finalizada la configuración, tendrá que cerrar la sesión y volver a iniciarla para administrar AD RMS.

13. Una vez que vuelva a iniciar sesión, Abra **Administrador del servidor** seleccione **herramientas** y, a continuación, **Active Directory Rights Management**. La ventana de administración debe aparecer e indicar que el clúster tiene el servidor adicional en el clúster.

14. Después de confirmar la configuración del servidor, configure el servicio de equilibrio de carga para equilibrar la carga entre los distintos servidores AD RMS del clúster.

#### <a name="adding-a-windows-server-2016-ad-fs-server-for-high-availability"></a>Agregar un servidor de AD FS de Windows Server 2016 para alta disponibilidad

Puede implementar más servidores de AD FS para configurar la alta disponibilidad. Puede optar por realizar esta acción en caso de que se aumente el tráfico a los servidores de AD FS. **Nota: después de elevar el nivel de comportamiento de la granja, se insertará una nueva entrada de base de datos en SQL Server 2016 (ADFS Configv3) y la base de datos de configuración anterior debe eliminarse antes de continuar con estos pasos.**

**Para agregar el servidor de AD FS de Windows Server 2016 para alta disponibilidad**

1.  Instale el rol de AD RMS en la implementación de Windows Server 2016 deseada.

2.  Una vez finalizada la instalación, seleccione el vínculo para **configurar el servicio de Federación en este servidor**.

3.  En la sección de bienvenida del asistente, elija la opción para **Agregar un servidor de Federación a una granja de servidores de Federación** y, a continuación, haga clic en **siguiente**.

4.  Especifique la cuenta de administrador adecuada y haga clic en **siguiente**.

5.  En la página **especificar granja de servidores** , seleccione la **Ubicación de la base de datos para especificar una granja de servidores existente mediante SQL Server** escriba el CNAME del servicio SQL para el nombre de host de base de datos y haga clic en **siguiente**.

6.  En el área **especificar cuenta de servicio** del asistente, escriba las credenciales de la cuenta de servicio de AD FS y, a continuación, haga clic en **siguiente**.

7.  En **Opciones de revisión**, haga clic en **siguiente**.

8.  Haga clic en **configurar** cuando el botón esté disponible.

9.  Después de la configuración, reinicie la máquina.

10. Después de confirmar la instalación del servidor, equilibre la carga de los servidores AD FS según sea necesario.

#### <a name="adding-a-windows-server-2016-wap-server-for-high-availability"></a>Adición de un servidor WAP de Windows Server 2016 para alta disponibilidad

Puede implementar servidores WAP adicionales para configurar la alta disponibilidad. Puede optar por realizar esta acción en caso de que se aumente el tráfico a los servidores de AD RMS.

**Para agregar un servidor proxy de aplicación Web de Windows Server 2016 para alta disponibilidad**

1.  En el servidor que desea configurar como proxy de aplicación Web, vaya a la consola de Administrador del servidor y haga clic en **Agregar roles y características**.

2.  En el **Asistente para agregar roles y características**, haga clic en **siguiente** hasta llegar a la pantalla de selección de rol de servidor.

3.  En la pantalla Seleccionar roles de servidor, seleccione **acceso remoto**y, a continuación, haga clic en **siguiente** hasta que vuelva a la pantalla Seleccionar roles de servidor.

4.  En la pantalla Seleccionar roles de servidor, seleccione **proxy de aplicación web**, haga clic en **Agregar características**y, a continuación, haga clic en **siguiente**.

5.  En la pantalla confirmar selecciones de instalación, haga clic en **instalar**.

6.  Una vez finalizada la instalación, haga clic en **cerrar**.

7.  Ahora es el momento de configurar el servidor. Para ello, abra la consola de administración de acceso remoto en el servidor proxy de aplicación Web. Abra el menú **Inicio** , escriba **RAMgmtUI. exe**y, a continuación, seleccione la aplicación.

8.  En el panel de navegación, haz clic en **Proxy de aplicación web**.

9.  En la consola de administración de acceso remoto, haga clic en **ejecutar el Asistente para configuración de proxy de aplicación web**. Una vez en el asistente, haga clic en **siguiente**.

10. En la pantalla servidor de Federación, escriba el nombre de dominio completo del servidor de AD FS (por ejemplo, adfs.contoso.com) y, a continuación, escriba las credenciales de un administrador en el servidor de AD FS.

11. En la pantalla certificado de proxy de AD FS, en la lista de certificados instalados actualmente en el servidor proxy de aplicación Web, seleccione el certificado que usará el proxy de aplicación web para AD FS proxy y, a continuación, haga clic en **siguiente**.

12. En la pantalla confirmación, revise la configuración y haga clic en **configurar**.

13. Una vez completada la configuración, haga clic en **cerrar**.

14. Después de confirmar la configuración del servidor, equilibre la carga de los servidores WAP en la red perimetral.

#### <a name="adding-a-sql-server-2016-node-for-always-on-high-availability"></a>Adición de un nodo SQL Server 2016 para la alta disponibilidad de Always On

Puede implementar más servidores SQL Server para configurar Always On alta disponibilidad. Puede optar por realizar esta acción en caso de que se aumente el tráfico a los servidores de AD RMS. **Nota: Asegúrese de que ambos servidores SQL tienen el puerto de entrada 5022 abierto.**

**Para agregar un servidor de SQL Server 2016 para obtener Always On alta disponibilidad**

1.  En el servidor que desea configurar como servidor SQL Server 2016 adicional, vaya a la consola de Administrador del servidor y haga clic en **Agregar roles y características**.

2.  Haga clic en **siguiente** hasta el cuadro de diálogo **seleccionar características** .

3.  Active la casilla **clúster de conmutación por error** . **Nota: siga este paso también para el servidor SQL Server 2016 original, de modo que ambos servidores SQL Server tengan la característica de clúster de conmutación por error.**

4.  Haga clic en **instalar** para instalar la característica de clúster de conmutación por error.

5.  Ahora, Abra **Administrador del servidor** y seleccione **herramientas** y, a continuación, **Administrador de clústeres de conmutación por error**.

6.  En el panel de menú izquierdo, haga clic con el botón derecho en **Administrador de clústeres de conmutación por error** y seleccione **crear clúster** .

7.  Se abrirá el **Asistente para crear clúster**.

8.  Busque los servidores de SQL Server 2016 que se usarán para Always On alta disponibilidad y escríbalos en y, a continuación, haga clic en **siguiente**.

9.  Recibirá una advertencia de validación. Seleccione **sí** para validar los nodos del clúster y, a continuación, haga clic en **siguiente**.

10. En la página **Opciones de prueba** , seleccione la opción **ejecutar todas las pruebas** y haga clic en **siguiente.**

11. **Nota: se espera que el Asistente para validación de clústeres devuelva varios mensajes de advertencia, especialmente si no va a usar el almacenamiento compartido. Aparte de eso, si encuentra algún mensaje de error, debe corregirlo antes de crear el clúster de conmutación por error de Windows Server**.

12. En el cuadro de diálogo **punto de acceso para administrar el clúster** , escriba el nombre del clúster y la dirección IP virtual para el clúster de conmutación por error de Windows Server y, a continuación, haga clic en **siguiente**.

13. Compruebe que la configuración sea correcta en **Resumen** y haga clic en **Finalizar**.

14. De nuevo en el **Administrador de clústeres de conmutación por error,** haga clic con el botón derecho en el clúster y seleccione **más acciones** y, después, **configuración de cuórum de clúster** .

15. Haga clic en **siguiente** y seleccione la opción para **seleccionar el testigo de cuórum** y volver a visitar **siguiente** .

16. En la página **seleccionar testigo de cuórum** , seleccione la opción **configurar un testigo de recurso compartido de archivos** . A continuación, haga clic en **Siguiente**.

17. Seleccione **examinar** y busque la ruta de acceso del recurso compartido de archivos que desea utilizar en el cuadro de diálogo ruta de acceso del recurso compartido de archivos. Haga clic en **Siguiente**.

18. En la página Confirmación, haga clic en **siguiente**.

19. En la página Resumen, haga clic en **Finalizar**.

20. Ahora, abra el menú **Inicio** y busque **Administrador de configuración de SQL Server**.

21. Haga clic con el botón secundario en el nombre del SQL Server y seleccione **propiedades**.

22. En el cuadro de diálogo Propiedades, seleccione la pestaña **alta disponibilidad de AlwaysOn** . Active la casilla **Habilitar grupos de disponibilidad AlwaysOn** . Haga clic en **Aceptar**. **Nota: haga esto en ambos servidores SQL Server 2016.**

23. A continuación, reinicie el servicio SQL Server.

24. Ahora, abra el menú **Inicio** y busque **SQL Server Management Studio** y, en el panel de navegación izquierdo, haga clic con el botón secundario en **grupos de disponibilidad** y haga clic en **Asistente para nuevo grupo de disponibilidad** y luego haga clic en **siguiente**.

25. En la página **especificar nombre de grupo de disponibilidad** , seleccione un nombre de grupo (por ejemplo, SQLAvailabilityGroup2016). A continuación, haga clic en **Siguiente**.

26. En la sección **seleccionar bases de datos** , especifique las bases de datos. Después, haga clic en Siguiente. **Nota: puede que sea necesario realizar una copia de seguridad de algunas bases de datos de nuevo o poner en modo de recuperación completa**.

27. Una vez en la página **especificar réplicas** , haga clic en el botón **Agregar réplica** y elija el otro 2016 SQL Server.

28. Después de agregar el otro servidor, haga clic en las casillas de verificación y establezca el servidor secundario en una secundaria legible.

29. Vaya a la pestaña **extremos** y haga clic en la opción **Actualizar** . Además, también puede desplazarse por y asegurarse de que la misma cuenta de servicio se encuentra en el nodo principal y en el secundario.

30. Ahora, elija la pestaña **preferencias de copia de seguridad** y seleccione la opción **preferir secundaria** .

31. Vaya a la pestaña **agente de escucha** .

32. Especifique un nombre (por ejemplo, SQLListener) y asegúrese de que el puerto es **1433** y, a continuación, haga clic en **siguiente**.

33. En la página **seleccionar sincronización de datos iniciales** del asistente, elija la opción **completa** y especifique ubicación de red accesible para todos los servidores SQL Server y, a continuación, haga clic en **siguiente**.

34. Por último, haga clic en **Finalizar** y el proceso se completará.

### <a name="decommission-windows-server-2012-r2-nodes"></a>Retirar nodos de Windows Server 2012 R2

En las secciones siguientes se proporcionan instrucciones sobre las tareas operativas que puede necesitar para quitar los servidores de Windows Server 2012 R2 después de actualizar correctamente el clúster de AD RMS a Windows Server 2016.

#### <a name="removing-a-windows-server-2012-r2-ad-rms-server"></a>Quitar un servidor de AD RMS de Windows Server 2012 R2

Puede quitar servidores de AD RMS innecesarios después de una actualización. Puede optar por realizar esta acción cuando sea necesario para retirar AD RMS servidores.

**Para quitar un servidor de AD RMS de** **Windows Server 2012 R2**

1.  En el servidor de AD RMS de Windows Server 2012 R2 en Administrador del servidor, seleccione **administrar** en los menús de la parte superior derecha y, a continuación, elija **quitar roles y características**.

2.  El **Asistente para quitar roles y características** se abrirá y, en la pantalla **antes de comenzar** , haga clic en **siguiente**.

3.  En la pantalla **selección de servidor** , haga clic en **siguiente**.

4.  En la pantalla **roles de servidor** , desactive la casilla situada junto a **Active Directory Rights Management Services** y haga clic en **siguiente**.

5.  En la pantalla **características** , haga clic en **siguiente**.

6.  En la pantalla **confirmación** , haga clic en **quitar**.

7.  Cuando se complete, reinicie el servidor.

8.  Ahora puede apagar este servidor y reasignar los recursos según sea necesario.

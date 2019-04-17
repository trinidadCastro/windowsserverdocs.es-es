---
ms.assetid: 82918181-525d-4e93-af96-957dac6aedb6
title: "Apéndice B: configurar el entorno de prueba"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: deb08b0663e5f349df7cce51ddabd4aae7f624c5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-b-setting-up-the-test-environment"></a>Apéndice B: configurar el entorno de prueba

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se describe los pasos para crear un laboratorio de prácticas para probar el Control de acceso dinámico. Las instrucciones están diseñadas para seguir de forma secuencial porque hay muchos componentes que tienen dependencias.  
  
## <a name="prerequisites"></a>Requisitos previos  
**Requisitos de hardware y software**  
  
Requisitos para la configuración de laboratorio de prueba:  
  
-   Un servidor de host que ejecuta Windows Server 2008 R2 con SP1 y Hyper-V  
  
-   Una copia de la imagen ISO de Windows Server 2012  
  
-   Una copia de la imagen ISO del Windows 8  
  
-   Microsoft Office 2010  
  
-   Un servidor que ejecuta Microsoft Exchange Server 2003 o posterior  
  
Que necesitas para crear las máquinas virtuales siguientes para probar los escenarios de Control de acceso dinámico:  
  
-   DC1 (controlador de dominio)  
  
-   DC2 (controlador de dominio)  
  
-   Archivo1 (servidor de archivos y Active Directory Rights Management Services)  
  
-   SRV1 (servidor POP3 y SMTP)  
  
-   CLIENTE1 (equipo cliente con Microsoft Outlook)  
  
Las contraseñas de las máquinas virtuales deben ser las siguientes:  
  
-   BUILTIN\Administrator:pass@word1  
  
-   Contoso\Administrator:pass@word1  
  
-   Todas las demás cuentas:pass@word1  
  
## <a name="build-the-test-lab-virtual-machines"></a>Crear la prueba de máquinas virtuales de laboratorio  
  
### <a name="install-the-hyper-v-role"></a>Instalar el rol de Hyper-V  
Debes instalar el rol de Hyper-V en un equipo que ejecute Windows Server 2008 R2 con SP1.  
  
##### <a name="to-install-the-hyper-v-role"></a>Para instalar el rol de Hyper-V  
  
1.  Haz clic en **inicio**y, a continuación, haz clic en el administrador del servidor.  
  
2.  En el área de resumen de las funciones de la ventana principal del administrador del servidor, haz clic en **agregar Roles**.  
  
3.  En la **seleccionar Roles de servidor** página, haz clic en **Hyper-V**.  
  
4.  En la **crear redes virtuales** página, haz clic en uno o varios adaptadores de red si quieres que su conexión de red esté disponible para máquinas virtuales.  
  
5.  En la **Confirmar selecciones de instalación** página, haz clic en **instalar**.  
  
6.  El equipo debe reiniciarse para completar la instalación. Haz clic en **cerrar** para finalizar el asistente y, a continuación, haz clic en **Sí** para reiniciar el equipo.  
  
7.  Después de reiniciar el equipo, inicia sesión con la misma cuenta que usaste para instalar el rol. Después de que el Asistente para configuración de reanudación se complete la instalación, haz clic en **cerrar** para finalizar el asistente.  
  
### <a name="create-an-internal-virtual-network"></a>Crear una red virtual interna  
Ahora vas a crear una red virtual interna llamada ID_AD_Network.  
  
##### <a name="to-create-a-virtual-network"></a>Para crear una red virtual  
  
1.  Abre el Administrador de Hyper-V.  
  
2.  Desde el **acciones** menú, haz clic en **Administrador de redes virtuales**.  
  
3.  En **crear red virtual**, selecciona el **interna**.  
  
4.  Haz clic en **agregar**. La **nueva red Virtual** aparecerá la página.  
  
5.  Tipo **ID_AD_Network** como nombre de la nueva red. Revisa las otras propiedades y modificarla si es necesario.  
  
6.  Haz clic en **Aceptar** para crear la red virtual y cerrar el Administrador de red virtuales o haz clic en **aplicar** para crear la red virtual y seguir usando el Administrador de red Virtual.  
  
### <a name="BKMK_Build"></a>Crear el controlador de dominio  
Crear una máquina virtual que se usará como el controlador de dominio (DC1). Instalar la máquina virtual con ISO de Windows Server 2012 y asígnale el nombre DC1.  
  
##### <a name="to-install-active-directory-domain-services"></a>Para instalar servicios de dominio de Active Directory  
  
1.  Conecta la máquina virtual a la ID_AD_Network. Inicia sesión en el DC1 como administrador con la contraseña ** pass@word1 **.  
  
2.  En el administrador del servidor, haz clic en **administrar**y, a continuación, haz clic en **agregar Roles y características**.  
  
3.  En la **antes de comenzar** página, haz clic en **siguiente**.  
  
4.  En la **selecciona el tipo de instalación** página, haz clic en **instalación basada en rol o característica**y, a continuación, haz clic en **siguiente**.  
  
5.  En la **servidor de destino selecciona** página, haz clic en **siguiente**.  
  
6.  En la **seleccionar roles de servidor** página, haz clic en **los servicios de dominio de Active Directory**. En la **agregar Roles and Features Wizard** cuadro de diálogo, haz clic en **agregar características**y, a continuación, haz clic en **siguiente**.  
  
7.  En la **Select features** página, haz clic en **siguiente**.  
  
8.  En la **los servicios de dominio de Active Directory** página, revisa la información y, a continuación, haz clic en **siguiente**.  
  
9. En la **Confirmar selecciones de instalación** página, haz clic en **instalar**. La barra de progreso de la instalación de características en la página de resultados indica que se instala el rol.  
  
10. En la **resultados** página, comprueba que la instalación se realizó correctamente y haz clic en **cerrar**. En el administrador del servidor, haz clic en el icono de advertencia con un signo de exclamación en la esquina superior derecha de la pantalla, junto a **administrar**. En la lista de tareas, haz clic en el **promover este servidor a un controlador de dominio** vínculo.  
  
11. En la **configuración de implementación** página, haz clic en **agregar un bosque nuevo**, escribe el nombre del dominio raíz, **contoso.com**y, a continuación, haz clic en **siguiente**.  
  
12. En la **opciones del controlador de dominio** página, selecciona los niveles funcionales de bosque y dominio como Windows Server 2012, especificar la contraseña DSRM ** pass@word1 **y, a continuación, haz clic en **siguiente**.  
  
13. En la **opciones DNS** página, haz clic en **siguiente**.  
  
14. En la **opciones adicionales** página, haz clic en **siguiente**.  
  
15. En la **rutas de acceso** página, escribe las ubicaciones de la base de datos de Active Directory, archivos de registro y carpeta SYSVOL (o Aceptar las ubicaciones predeterminadas) y, a continuación, haz clic en **siguiente**.  
  
16. En la **opciones de revisión** página, confirma las opciones seleccionadas y, a continuación, haz clic en **siguiente**.  
  
17. En la **comprobación de requisitos previos** página, confirma que se complete la validación de los requisitos previos y, a continuación, haz clic en **instalar**.  
  
18. En la **resultados** página, comprueba que el servidor se ha configurado correctamente como un controlador de dominio y, a continuación, haz clic en **cerrar**.  
  
19. Reiniciar el servidor para completar la instalación de AD DS. (De manera predeterminada, esto se produce automáticamente.)  
  
Crear los siguientes usuarios mediante el centro de administración de Active Directory.  
  
##### <a name="create-users-and-groups-on-dc1"></a>Crear los usuarios y grupos en DC1  
  
1.  Inicia sesión en contoso.com como administrador. Iniciar el centro de administración de Active Directory.  
  
2.  Crear los siguientes grupos de seguridad:  
  
    |Nombre del grupo|Dirección de correo electrónico|  
    |--------------|-----------------|  
    |FinanceAdmin|financeadmin@contoso.com|  
    |FinanceException|financeexception@contoso.com|  
  
3.  Crea la siguiente unidad organizativa (OU):  
  
    |Nombre de UO|Equipos|  
    |-----------|-------------|  
    |FileServerOU|ARCHIVO1|  
  
4.  Crear los siguientes usuarios con los atributos que se indica:  
  
    |Usuario|Nombre de usuario|Dirección de correo electrónico|Departamento|Grupo|País o región|  
    |--------|------------|-----------------|--------------|---------|-------------------|  
    |Myriam Delesalle|MDelesalle|MDelesalle@contoso.com|Finanzas||NOS|  
    |Hernández|MReid|MReid@contoso.com|Finanzas|FinanceAdmin|NOS|  
    |Esther Valle|EValle|EValle@contoso.com|Operaciones|FinanceException|NOS|  
    |Maira Wenzel|MWenzel|MWenzel@contoso.com|HR||NOS|  
    |Jeff bajo|JLow|JLow@contoso.com|HR||NOS|  
    |Servidor RMS|RMS|rms@contoso.com||||  
  
    Para obtener más información sobre la creación de grupos de seguridad, consulta [crear un nuevo grupo](https://technet.microsoft.com/library/dd861305.aspx) en el sitio Web de Windows Server.  
  
##### <a name="to-create-a-group-policy-object"></a>Para crear un objeto de directiva de grupo  
  
1.  Mantener el cursor en la esquina superior derecha de la pantalla y haz clic en el icono de búsqueda. En el cuadro de búsqueda, escriba **administración de directivas de grupo**y haz clic en **Group Policy Management**.  
  
2.  Expande **bosque: contoso.com**y después expande **dominios**, navegar a **contoso.com**, expanda **(contoso.com)**y, a continuación, selecciona **FileServerOU**. Haz clic en **crear un GPO en este dominio y vincularlo aquí**
  
3.  Escribe un nombre descriptivo para el GPO, como **FlexibleAccessGPO**y, a continuación, haz clic en **Aceptar**.  
  
##### <a name="to-enable-dynamic-access-control-for-contosocom"></a>Para habilitar el Control de acceso dinámico para contoso.com  
  
1.  Abre la consola de administración de directivas de grupo, haz clic en **contoso.com**y, a continuación, haz doble clic en **controladores de dominio**.  
  
2.  Haz clic en **directiva predeterminada de controladores de dominio**y selecciona **editar**.  
  
3.  En la ventana del Editor de administración de directivas de grupo, haz doble clic en **configuración del equipo**, haz doble clic en **directivas**, haz doble clic en **plantillas administrativas**, haz doble clic en **sistema**y, a continuación, haz doble clic en **KDC**.  
  
4.  Haz doble clic en **compatibilidad de KDC con notificaciones, autenticación compuesta y protección de Kerberos** y selecciona la opción junto a **habilitado**. Debes habilitar esta opción para usar directivas de acceso Central.  
  
5.  Abre un símbolo del sistema con privilegios elevados y ejecuta el siguiente comando:  
  
    ```  
    gpupdate /force  
    ```  
  
### <a name="BKMK_FS1"></a>Crear el servidor de archivos y el servidor de AD RMS (archivo1)  
  
1.  Crear una máquina virtual con el nombre archivo1 desde la imagen ISO de Windows Server 2012.  
  
2.  Conecta la máquina virtual a la ID_AD_Network.  
  
3.  Unirse a la máquina virtual al dominio contoso.com y, a continuación, inicia sesión en archivo1 como Contoso\Administrador con la contraseña ** pass@word1 **.  
  
#### <a name="install-file-services-resource-manager"></a>Instalar el Administrador de recursos de servicios de archivos  
  
###### <a name="to-install-the-file-services-role-and-the-file-server-resource-manager"></a>Para instalar el rol de servicios de archivo y el Administrador de recursos del servidor de archivos  
  
1.  En el administrador del servidor, haz clic en **agregar Roles y características**.  
  
2.  En la **antes de comenzar** página, haz clic en **siguiente**.  
  
3.  En la **selecciona el tipo de instalación** página, haz clic en **siguiente**.  
  
4.  En la **servidor de destino selecciona** página, haz clic en **siguiente**.  
  
5.  En la **seleccionar Roles de servidor** , expanda **File and Storage Services**, selecciona la casilla de verificación junto a **iSCSI y archivo servicios**, expande y selecciona **Administrador de recursos del servidor de archivos**.  
  
    En el agregar Roles y características de asistente, haz clic en **agregar características**y, a continuación, haz clic en **siguiente**.  
  
6.  En la **Select features** página, haz clic en **siguiente**.  
  
7.  En la **Confirmar selecciones de instalación** página, haz clic en **instalar**.  
  
8.  En la **progreso de la instalación** página, haz clic en **cerrar**.  
  
#### <a name="install-the-microsoft-office-filter-packs-on-the-file-server"></a>Instalar los paquetes de filtro de Microsoft Office en el servidor de archivos  
Debes instalar los paquetes de filtro de Microsoft Office en Windows Server 2012 para habilitar IFilters para una mayor variedad de archivos de Office que se proporcionan de manera predeterminada.  Windows Server 2012 no tiene ningún IFilters para archivos de Microsoft Office instalado de manera predeterminada, y la infraestructura de clasificación de archivo usa IFilters para realizar un análisis de contenido.  
  
Para descargar e instalar los IFilters, consulta [paquetes de filtro de Microsoft Office 2010](https://go.microsoft.com/fwlink/?LinkID=234122).  
  
#### <a name="configure-email-notifications-on-file1"></a>Configurar notificaciones por correo electrónico en archivo1  
Cuando creas las cuotas y filtros de archivos, tienes la opción de enviar notificaciones por correo electrónico a los usuarios cuando se está acercando a su límite de cuota o después de que han intentado guardar los archivos que se han bloqueado. Si desea notificar determinados administradores de cuota de filtrado y eventos de forma rutinaria, puedes configurar a uno o más destinatarios de forma predeterminada. Para enviar estas notificaciones, debes especificar el servidor SMTP que se usará para reenviar los mensajes de correo electrónico.  
  
###### <a name="to-configure-email-options-in-file-server-resource-manager"></a>Para configurar las opciones de correo electrónico en el Administrador de recursos del servidor de archivos  
  
1.  Abre el Administrador de recursos del servidor de archivos. Para abrir el Administrador de recursos del servidor de archivos, haz clic en **inicio**, tipo **Administrador de recursos del servidor de archivos**y, a continuación, haz clic en **Administrador de recursos del servidor de archivos**.  
  
2.  En la interfaz de administrador de recursos del servidor de archivos, haz clic en **Administrador de recursos del servidor de archivos**y, a continuación, haz clic en **configurar las opciones de**. La **opciones de administrador de recursos del servidor de archivos** abre el cuadro de diálogo.  
  
3.  En la **notificaciones por correo electrónico** ficha, en nombre del servidor SMTP o dirección IP, escriba el nombre de host o la dirección IP del servidor SMTP que enviará notificaciones por correo electrónico.  
  
4.  Si quieres notificar determinados administradores de cuota de forma rutinaria o eventos del detección, de archivos en **Administradores receptores predeterminados**, escribe cada dirección de correo electrónico como fileadmin@contoso.com. Usar el formato account@domainy usa el punto y coma para separar varias cuentas.  
  
#### <a name="create-groups-on-file1"></a>Crear grupos en archivo1  
  
###### <a name="to-create-security-groups-on-file1"></a>Para crear grupos de seguridad en archivo1  
  
1.  Inicia sesión en archivo1 como Contoso\Administrador con la contraseña: ** pass@word1 **.  
  
2.  Agregue NT Authority\usuarios a la **WinRMRemoteWMIUsers__** grupo.  
  
#### <a name="create-files-and-folders-on-file1"></a>Crear archivos y carpetas en archivo1  
  
1.  Crear un nuevo volumen NTFS en archivo1 y, a continuación, crea la siguiente carpeta: D:\Finance documentos.  
  
2.  Crea los siguientes archivos con los detalles que especifica:  
  
    -   **Finanzas Memo.docx**: agregar algunas financiación a texto relacionados en el documento. Por ejemplo, ' han cambiado las reglas de negocio sobre quiénes pueden acceder documentos de finanzas. Finanzas documentos se ahora solo tiene acceso a los miembros del grupo FinanceExpert. No hay otros departamentos o grupos tienen acceso.' Debes evaluar el impacto de este cambio antes de implementarla en el entorno. Asegúrate de que este documento tenga CONTOSO confidencial como pie de página en cada página.  
  
    -   **Solicitud de aprobación para Hire.docx**: crear un formulario en este documento que recopila información del candidato. Debes tener los siguientes campos en el documento: **candidato, número de la seguridad Social, puesto de trabajo, propuesta sueldo, fecha inicial, nombre de Supervisor, departamento**. Agregar una sección adicional en el documento que tiene un formulario para **firma Supervisor, sueldo aprobados, confirmación de ofrecer**, y **estado ofrecen**.   
        Hacer que el documento administración de derechos habilitada.  
  
    -   **Word Document1.docx**: agregar algún contenido de prueba en este documento.  
  
    -   **Word Document2.docx**: agregar contenido a este documento de prueba.  
  
    -   **Workbook1.xlsx**  
  
    -   **Workbook2.xlsx**  
  
    -   Crea una carpeta en el escritorio denominado expresiones regulares. Crear un documento de texto en la carpeta denominada **RegEx NSS**. Escribe el siguiente contenido en el archivo y, a continuación, guarda y cierra el archivo:   
        ^(?! 000)([0-7]\d{2}|7([0-7]\d|7[012])) ([-]?) (?! 00) \d\d\3 (! 0000) \d {4} $  
  
3.  Comparte la carpeta documentos de D:\Finance como documentos de finanzas y permitir a todos tienen lectura y escritura de acceso al recurso compartido.  
  
> [!NOTE]  
> Directivas de acceso central no están habilitadas de manera predeterminada en el sistema o el volumen C: de arranque.  
  
#### <a name="BKMK_CS1"></a>Instalar Active Directory Rights Management Services  
Agrega el Active Directory Rights Management Services (AD RMS) y todas las características requeridas mediante el administrador del servidor. Seleccionar todos los valores predeterminados.  
  
###### <a name="to-install-active-directory-rights-management-services"></a>Para instalar Active Directory Rights Management Services  
  
1.  Inicia sesión en el archivo1 como Contoso\Administrador o como un miembro del grupo Administradores de dominio.  
  
    > [!IMPORTANT]  
    > Para instalar el rol de servidor de AD RMS el instalador de cuenta (en este caso, CONTOSO\Administrator) tendrá que dar pertenencia tanto el grupo de administradores local en el equipo servidor sea de AD RMS para instalarse, así como la pertenencia al grupo de administradores de empresa en Active Directory.  
  
2.  En el administrador del servidor, haz clic en **agregar Roles y características**. Aparece el agregar Roles y características de asistente.  
  
3.  En la **antes de comenzar** de pantalla, haz clic en **siguiente**.  
  
4.  En la **seleccionar el tipo de instalación** de pantalla, haz clic en **según instalar el rol o característica**y, a continuación, haz clic en **siguiente**.  
  
5.  En la **selecciona destinos del servidor** de pantalla, haz clic en **siguiente**.  
  
6.  En la **seleccionar Roles de servidor** de pantalla, active la casilla junto a **Active Directory Rights Management Services**y, a continuación, haz clic en **siguiente**.  
  
7.  ¿En la **agregar características que son necesarias para Active Directory Rights Management Services? ** cuadro de diálogo, haz clic en **agregar características**.  
  
8.  En la **seleccionar Roles de servidor** de pantalla, haz clic en **siguiente**.  
  
9. En la **selecciona funciones que se instalarán** de pantalla, haz clic en **siguiente**.  
  
10. En la **Active Directory Rights Management Services** de pantalla, haz clic en siguiente.  
  
11. En la **seleccionar servicios de rol** de pantalla, haz clic en **siguiente**.  
  
12. En la **rol de servidor Web (IIS)** de pantalla, haz clic en **siguiente**.  
  
13. En la **seleccionar servicios de rol** de pantalla, haz clic en **siguiente**.  
  
14. En la **Confirmar selecciones de instalación** de pantalla, haz clic en **instalar**.  
  
15. Cuando haya finalizado la instalación, en la **progreso de la instalación** de pantalla, haz clic en **realizar una configuración adicional**. Aparece el Asistente para la configuración de AD RMS.  
  
16. En la **AD RMS** de pantalla, haz clic en **siguiente**.  
  
17. En la **clúster de AD RMS** pantalla, selecciona **crear un nuevo clúster raíz de AD RMS** y, a continuación, haz clic en **siguiente**.  
  
18. En la **base de datos de configuración** de pantalla, haz clic en **usa Windows Internal Database en este servidor**y, a continuación, haz clic en **siguiente**.  
  
    > [!NOTE]  
    > Se recomienda usar la base de datos interna de Windows en entornos de prueba, ya que no admite más de un servidor en el clúster de AD RMS. Las implementaciones de producción deben usar un servidor de bases de datos independiente.  
  
19. En la **cuenta de servicio** de pantalla, en **cuenta de usuario de dominio**, haz clic en **especificar** y, a continuación, especifica el nombre de usuario (**contoso\rms**) y la contraseña (**pass@word1**) y haz clic en **Aceptar**y, a continuación, haz clic en **siguiente**.  
  
20. En la **modo criptográfico** de pantalla, haz clic en **criptográfica de modo de 2**.  
  
21. En la **almacenamiento de claves de clúster** de pantalla, haz clic en **siguiente**.  
  
22. En la **contraseña de la clave de clúster** de pantalla, en la **contraseña** y **Confirmar contraseña** cuadros, escribe ** pass@word1 **y, a continuación, haz clic en **siguiente**.  
  
23. En la **sitio Web de clúster** de pantalla, asegúrate de que **Default Web Site** está seleccionado y, a continuación, haz clic en **siguiente**.  
  
24. En la **dirección clúster** pantalla, selecciona el **usar una conexión sin cifrar** opción, en la **nombre de dominio completo** , escriba **FILE1.contoso.com**y, a continuación, haz clic en **siguiente**.  
  
25. En la **nombre del certificado Licenciante** de pantalla, acepta el nombre predeterminado (**archivo1**) en el cuadro de texto y haz clic en **siguiente**.  
  
26. En la **registro** pantalla, selecciona **registrar SCP ahora**y, a continuación, haz clic en **siguiente**.  
  
27. En la **confirmación** de pantalla, haz clic en **instalar**.  
  
28. En la **resultados** de pantalla, haz clic en **cerrar**y, a continuación, haz clic en **cerrar** en **progreso de la instalación** pantalla. Cuando termine, cierra la sesión e iniciar sesión como contoso\rms con la contraseña proporcionada (**pass@word1**).  
  
29. Iniciar la consola de AD RMS y navegar a **plantillas de directiva de derechos**.  
  
    Para abrir la consola de AD RMS, en el administrador del servidor, haz clic en **servidor Local** en el árbol de consola, a continuación, haz clic en **herramientas**y, a continuación, haz clic en **Active Directory Rights Management Services**.  
  
30. Haz clic en el **Crear directiva de derechos distribuida** plantilla ubicada en el panel derecho, haz clic en **agregar**y selecciona la siguiente información:  
  
    -   Idioma: Inglés de Estados Unidos  
  
    -   Nombre: Solo administración de finanzas de Contoso  
  
    -   Description: Solo administración de finanzas de Contoso  
  
    Haz clic en **agregar**y, a continuación, haz clic en **siguiente**.  
  
31. En la sección de usuarios y derechos, haz clic en **usuarios y derechos**, haz clic en **agregar**, tipo ** financeadmin@contoso.com **y haz clic en **Aceptar**.  
  
32. Selecciona **Control total**y deja **conceder propietario (autor) completa derecho de control no tienen expiración** seleccionado.  
  
33. Haz clic en las pestañas sin cambios restantes aunque y, a continuación, haz clic en **finalizar**. Inicia sesión como Contoso\Administrador.  
  
34. Busca el Seleccionar carpeta, C:\inetpub\wwwroot\\_wmcs\certification, el archivo ServerCertification.asmx y agregar usuarios autenticados para dispone de permisos lectura y escritura al archivo.  
  
35. Abre Windows PowerShell y ejecuta `Get-FsrmRmsTemplate`. Comprueba que es posible ver la plantilla de RMS que creaste en los pasos anteriores en este procedimiento con este comando.  
  
> [!IMPORTANT]  
> Si quieres que los servidores de archivos para cambiar inmediatamente para que puedas probar ellos, debes hacer lo siguiente:  
>   
> 1.  En el servidor de archivos, archivo1, abre un símbolo del sistema con privilegios elevados y ejecuta los siguientes comandos:  
>   
>     -   comando de gpupdate/force.  
>     -   NLTEST /SC_RESET:contoso.com  
> 2.  En el controlador de dominio (DC1), replicar Active Directory.  
>   
>     Para obtener más información acerca de los pasos para forzar la replicación de Active Directory, consulte [replicación de Active Directory](https://technet.microsoft.com/library/cc794809(WS.10).aspx)  
  
Opcionalmente, en lugar de usar la agregar Roles y características de asistente en el administrador del servidor, puedes usar Windows PowerShell para instalar y configurar el rol de servidor de AD RMS, como se muestra en el siguiente procedimiento.  
  
###### <a name="to-install-and-configure-an-ad-rms-cluster-in-windows-server-2012-using-windows-powershell"></a>Para instalar y configurar un clúster de AD RMS en Windows Server 2012 mediante Windows PowerShell  
  
1.  Inicio de sesión como Contoso\Administrador con la contraseña: ** pass@word1 **.  
  
    > [!IMPORTANT]  
    > Para instalar el rol de servidor de AD RMS el instalador de cuenta (en este caso, CONTOSO\Administrator) tendrá que dar pertenencia tanto el grupo de administradores local en el equipo servidor sea de AD RMS para instalarse, así como la pertenencia al grupo de administradores de empresa en Active Directory.  
  
2.  En el escritorio, haz clic en el icono de Windows PowerShell en la barra de tareas y selecciona **ejecutar como administrador** para abrir un símbolo del sistema de Windows PowerShell con privilegios administrativos.  
  
3.  Para usar los cmdlets de administrador del servidor para instalar el rol de servidor de AD RMS, escribe:  
  
    ```  
    Add-WindowsFeature ADRMS '"IncludeAllSubFeature '"IncludeManagementTools  
    ```  
  
4.  Crear la unidad de Windows PowerShell para representar el servidor de AD RMS que vas a instalar.  
  
    Por ejemplo, para crear una unidad de Windows PowerShell denominada RC para instalar y configurar el primer servidor en un clúster de raíz de AD RMS, escribe:  
  
    ```  
    Import-Module ADRMS  
    New-PSDrive -PSProvider ADRMSInstall -Name RC -Root RootCluster  
    ```  
  
5.  Establecer propiedades en objetos en el espacio de nombres de la unidad que representan las opciones de configuración necesarias.  
  
    Por ejemplo, para establecer la cuenta de servicio de AD RMS, en el símbolo del sistema de Windows PowerShell, escribe:  
  
    ```  
    $svcacct = Get-Credential  
    ```  
  
    Cuando aparezca el cuadro de diálogo de seguridad de Windows, escribe el nombre de usuario de dominio de AD RMS servicio cuenta CONTOSO\RMS y la contraseña asignada.  
  
    A continuación, para asignar la cuenta de servicio de AD RMS a la configuración de clúster de AD RMS, escribe lo siguiente:  
  
    ```  
    Set-ItemProperty -Path RC:\ -Name ServiceAccount -Value $svcacct  
    ```  
  
    A continuación, para establecer el servidor de AD RMS para usar la base de datos interna de Windows, en el símbolo del sistema de Windows PowerShell, escribe:  
  
    ```  
    Set-ItemProperty -Path RC:\ClusterDatabase -Name UseWindowsInternalDatabase -Value $true  
    ```  
  
    A continuación, para almacenar la contraseña de la clave de clúster de forma segura en una variable, en el símbolo del sistema de Windows PowerShell, escribe:  
  
    ```  
    $password = Read-Host -AsSecureString -Prompt "Password:"  
    ```  
  
    Escribe la contraseña de la clave de clúster y, a continuación, presione la tecla ENTRAR.  
  
    A continuación, para asignar la contraseña para la instalación de AD RMS, en el símbolo del sistema de Windows PowerShell, escribe:  
  
    ```  
    Set-ItemProperty -Path RC:\ClusterKey -Name CentrallyManagedPassword -Value $password  
    ```  
  
    A continuación, para establecer la dirección de clúster de AD RMS, en el símbolo del sistema de Windows PowerShell, escribe:  
  
    ```  
    Set-ItemProperty -Path RC:\ -Name ClusterURL -Value "http://file1.contoso.com:80"  
    ```  
  
    A continuación, para asignar el nombre SLC para la instalación de AD RMS, en el símbolo del sistema de Windows PowerShell, escribe:  
  
    ```  
    Set-ItemProperty -Path RC:\ -Name SLCName -Value "FILE1"  
    ```  
  
    A continuación, para establecer el punto de conexión de servicio (SCP) para el clúster de AD RMS, en el símbolo del sistema de Windows PowerShell, escribe:  
  
    ```  
    Set-ItemProperty -Path RC:\ -Name RegisterSCP -Value $true  
    ```  
  
6.  Ejecutar el **instalación ADRMS** cmdlet. Además de instalar el rol de servidor de AD RMS y configurar el servidor, este cmdlet instala otras características de AD RMS requeridos si es necesario.  
  
    Por ejemplo, para cambiar a la unidad de Windows PowerShell denominada RC e instalar y configurar AD RMS, escribe:  
  
    ```  
    Set-Location RC:\  
    Install-ADRMS -Path.  
    ```  
  
    Escriba "Y" cuando el cmdlet le pedirá que confirme desea iniciar la instalación.  
  
7.  Cerrar sesión como Contoso\Administrador e iniciar sesión en como CONTOSO\RMS usando la contraseña proporcionada ("pass@word1").  
  
    > [!IMPORTANT]  
    > Para administrar el servidor de AD RMS la cuenta que ha iniciado sesión y usar para administrar el servidor (en este caso, CONTOSO\RMS) tendrá que dar pertenencia en ambas grupo de administradores locales en el equipo de servidor de AD RMS, así como la pertenencia al grupo de administradores de empresa en Active Directory.  
  
8.  En el escritorio, haz clic en el icono de Windows PowerShell en la barra de tareas y selecciona **ejecutar como administrador** para abrir un símbolo del sistema de Windows PowerShell con privilegios administrativos.  
  
9. Crear la unidad de Windows PowerShell para representar el servidor de AD RMS que va a configurar.  
  
    Por ejemplo, para crear una unidad de Windows PowerShell denominada RC para configurar el clúster de raíz de AD RMS, escribe:  
  
    ```  
    Import-Module ADRMSAdmin `  
    New-PSDrive -PSProvider ADRMSAdmin -Name RC -Root http://localhost -Force -Scope Global  
    ```  
  
10. Para crear la nueva plantilla de derechos para el Administrador de finanzas de Contoso y asignar derechos de usuario con el control total de la instalación de AD RMS, en el símbolo del sistema de Windows PowerShell, escribe:  
  
    ```  
    New-Item -Path RC:\RightsPolicyTemplate '"LocaleName en-us -DisplayName "Contoso Finance Admin Only" -Description "Contoso Finance Admin Only" -UserGroup financeadmin@contoso.com  -Right ('FullControl')  
    ```  
  
11. Para comprobar que puedes ver la nueva plantilla de derechos para el Administrador de finanzas de Contoso, en el símbolo del sistema de Windows PowerShell:  
  
    ```  
    Get-FsrmRmsTemplate  
    ```  
  
    Revisa el resultado de este cmdlet para confirmar la plantilla de RMS que creaste en el paso anterior está presente.  
  
### <a name="build-the-mail-server-srv1"></a>Crear el servidor de correo (SRV1)  
SRV1 es el servidor de correo electrónico POP3/SMTP. Debes configurarlo para que pueda enviar notificaciones por correo electrónico como parte del escenario de asistencia de acceso denegado.  
  
Configurar Microsoft Exchange Server en este equipo. Para obtener más información, consulta [cómo instalar Exchange Server](https://go.microsoft.com/fwlink/?prd=12364).  
  
### <a name="build-the-client-virtual-machine-client1"></a>Crear la máquina virtual de cliente (CLIENTE1)  
  
##### <a name="to-build-the-client-virtual-machine"></a>Para crear la máquina virtual de cliente  
  
1.  Conecta el CLIENTE1 a la ID_AD_Network.  
  
2.  Instala Microsoft Office 2010.  
  
3.  Inicia sesión como Contoso\Administrador y usar la siguiente información para configurar Microsoft Outlook.  
  
    -   Tu nombre: Administrador de archivos  
  
    -   Dirección de correo electrónico:fileadmin@contoso.com  
  
    -   Tipo de cuenta: POP3  
  
    -   Servidor de correo entrante: dirección IP estática de SRV1  
  
    -   Servidor de correo saliente: dirección IP estática de SRV1  
  
    -   Nombre de usuario:fileadmin@contoso.com  
  
    -   Recordar contraseña: selecciona  
  
4.  Crear un acceso directo a Outlook en el escritorio contoso\administrator.  
  
5.  Abre Outlook y dirección de los mensajes 'inicia la primera vez'.  
  
6.  Eliminar los mensajes de prueba que se generaron.  
  
7.  Crear un nuevo método abreviado en el escritorio para todos los usuarios en la máquina virtual de cliente que señala a \\\FILE1\Finance documentos.  
  
8.  Reiniciar según sea necesario.  
  
##### <a name="enable-access-denied-assistance-on-the-client-virtual-machine"></a>Habilitar acceso denegado asistencia en la máquina virtual de cliente  
  
1.  Abre el Editor del registro y navegar a **HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Explorer**.  
  
    -   Establecer **EnableShellExecuteFileStreamCheck** a **1**.  
  
    -   Valor: DWORD  
  
## <a name="BKMK_CF"></a>Configuración de laboratorio para implementar notificaciones a través de escenario de bosques  
  
### <a name="BKMK_2.1"></a>Crear una máquina virtual para DC2  
  
-   Crear una máquina virtual desde la imagen ISO de Windows Server 2012.  
  
-   Crear el nombre de la máquina virtual como DC2.  
  
-   Conecta la máquina virtual a la ID_AD_Network.  
  
> [!IMPORTANT]  
> Máquinas virtuales de unirse a un dominio y la implementación de los tipos de notificación entre bosques requieren que las máquinas virtuales pueda resolver el FQDN de los dominios relevantes. Puede que configurar manualmente la configuración de DNS en las máquinas virtuales para lograr esto. Para obtener más información, consulta [configurar una red virtual](https://technet.microsoft.com/library/cc732470%28v=ws.10%29.aspx).  
>   
> Todas las imágenes de máquina virtual (clientes y servidores) deben configurarse para usar una dirección IP estática versión 4 (IPv4) dirección y la configuración de cliente de sistema de nombres de dominio (DNS). Para obtener más información, consulta [configurar un cliente DNS para la dirección IP estática](https://go.microsoft.com/fwlink/?LinkId=150952).  
  
### <a name="BKMK_2.2"></a>Configurar un bosque nuevo llamado adatum.com  
  
##### <a name="to-install-active-directory-domain-services"></a>Para instalar servicios de dominio de Active Directory  
  
1.  Conecta la máquina virtual a la ID_AD_Network. Inicia sesión en el DC2 como administrador con la contraseña ** Pass@word1 **.  
  
2.  En el administrador del servidor, haz clic en **administrar**y, a continuación, haz clic en **agregar Roles y características**.  
  
3.  En la **antes de comenzar** página, haz clic en **siguiente**.  
  
4.  En la **seleccionar el tipo de instalación** página, haz clic en **instalación basada en rol o característica**y, a continuación, haz clic en **siguiente**.  
  
5.  En la **servidor de destino selecciona** página, haz clic en **seleccionar un servidor desde el grupo de servidores**, haz clic en los nombres del servidor donde desea instalar servicios de dominio de Active Directory (AD DS) y, a continuación, haz clic en **siguiente**.  
  
6.  En la **seleccionar Roles de servidor** página, haz clic en **los servicios de dominio de Active Directory**. En la **agregar Roles and Features Wizard** cuadro de diálogo, haz clic en **agregar características**y, a continuación, haz clic en **siguiente**.  
  
7.  En la **seleccionar características** página, haz clic en **siguiente**.  
  
8.  En la **AD DS** página, revisa la información y, a continuación, haz clic en **siguiente**.  
  
9. En la **confirmación** página, haz clic en **instalar**. La barra de progreso de la instalación de características en la página de resultados indica que se instala el rol.  
  
10. En la **resultados** página, comprueba que la instalación se realizó correctamente y, a continuación, haz clic en el icono de advertencia con un signo de exclamación en la esquina superior derecha de la pantalla, junto a **administrar**. En la lista de tareas, haz clic en el **promover este servidor a un controlador de dominio** vínculo.  
  
    > [!IMPORTANT]  
    > Si cierra el Asistente para la instalación en este momento en lugar de haz clic en **promover este servidor a un controlador de dominio**, puedes seguir haciendo clic en la instalación de AD DS **tareas** en el administrador del servidor.  
  
11. En la **configuración de implementación** página, haz clic en **agregar un bosque nuevo**, escribe el nombre del dominio raíz, **adatum.com**y, a continuación, haz clic en **siguiente**.  
  
12. En la **opciones del controlador de dominio** página, selecciona los niveles funcionales de bosque y dominio como Windows Server 2012, especificar la contraseña DSRM ** pass@word1 **y, a continuación, haz clic en **siguiente**.  
  
13. En la **opciones DNS** página, haz clic en **siguiente**.  
  
14. En la **opciones adicionales** página, haz clic en **siguiente**.  
  
15. En la **rutas de acceso** página, escribe las ubicaciones de la base de datos de Active Directory, archivos de registro y carpeta SYSVOL (o Aceptar las ubicaciones predeterminadas) y, a continuación, haz clic en **siguiente**.  
  
16. En la **opciones de revisión** página, confirma las opciones seleccionadas y, a continuación, haz clic en **siguiente**.  
  
17. En la **comprobación de requisitos previos** página, confirma que se complete la validación de los requisitos previos y, a continuación, haz clic en **instalar**.  
  
18. En la **resultados** página, comprueba que el servidor se ha configurado correctamente como un controlador de dominio y, a continuación, haz clic en **cerrar**.  
  
19. Reiniciar el servidor para completar la instalación de AD DS. (De manera predeterminada, esto se produce automáticamente.)  
  
> [!IMPORTANT]  
> Para garantizar que la red está configurada correctamente, una vez haya configurado tanto los bosques, haz lo siguiente:  
>   
> -   Inicia sesión como adatum\administrator en adatum.com. Abre una ventana del símbolo del sistema, escribe **nslookup contoso.com**, y, a continuación, presione ENTRAR.  
> -   Inicia sesión como Contoso\Administrador en contoso.com. Abre una ventana del símbolo del sistema, escribe **nslookup adatum.com**, y, a continuación, presione ENTRAR.  
>   
> Si estos comandos se ejecutan sin errores, los bosques pueden comunicarse entre sí. Para obtener más información sobre los errores de nslookup, consulta la sección de solución de problemas en el tema [utilizar NSlookup.exe](https://support.microsoft.com/kb/200525)  
  
### <a name="BKMK_2.22"></a>Establecer contoso.com como un bosque de confianza a adatum.com  
En este paso, crearás una relación de confianza entre el sitio Adatum Corporation y el sitio de Contoso, Ltd..  
  
##### <a name="to-set-contoso-as-a-trusting-forest-to-adatum"></a>Para configurar Contoso como un bosque de confianza a Adatum  
  
1.  Inicia sesión como administrador en DC2. En la **inicio** , escriba domain.msc.  
  
2.  En el árbol de consola, haz clic en adatum.com y, a continuación, haga clic en Propiedades.  
  
3.  En la **confía** , haga clic **nueva confianza**y, a continuación, haz clic en **siguiente**.  
  
4.  En la **nombre de confianza** , escriba **contoso.com**, campo de nombre de sistema de nombres de dominio (DNS) y, a continuación, haz clic en **siguiente**.  
  
5.  En la **tipo de confianza** página, haz clic en **confianza de bosque**y, a continuación, haz clic en **siguiente**.  
  
6.  En la **dirección de confianza** página, haz clic en **bidireccional**.  
  
7.  En la **lados de confianza** página, haz clic en **ambos, este dominio y el dominio especificado**y, a continuación, haz clic en **siguiente**.  
  
8.  Sigue las instrucciones en el Asistente para continuar.  
  
### <a name="BKMK_2.4"></a>Crear usuarios adicionales en el bosque Adatum  
Crear el usuario Jeff Low con la contraseña ** pass@word1 **y asigna el atributo de empresa con el valor **Adatum**.  
  
##### <a name="to-create-a-user-with-the-company-attribute"></a>Para crear un usuario con el atributo de empresa  
  
1.  Abre un símbolo del sistema con privilegios elevados en Windows PowerShell y pega el siguiente código:  
  
    ```  
    New-ADUser `  
    -SamAccountName jlow `  
    -Name "Jeff Low" `  
    -UserPrincipalName jlow@adatum.com `  
    -AccountPassword (ConvertTo-SecureString `  
    -AsPlainText "pass@word1" -Force) `  
    -Enabled $true `  
    -PasswordNeverExpires $true `  
    -Path 'CN=Users,DC=adatum,DC=com' `  
    -Company Adatum`  
  
    ```  
  
### <a name="BKMK_2.5"></a>Crear el tipo de notificación de la empresa en adataum.com  
  
##### <a name="to-create-a-claim-type-by-using-windows-powershell"></a>Para crear un tipo de notificación mediante Windows PowerShell  
  
1.  Inicia sesión como administrador en adatum.com.  
  
2.  Abre un símbolo del sistema con privilegios elevados en Windows PowerShell y escribe el siguiente código:  
  
    ```  
    New-ADClaimType `  
    -AppliesToClasses:@('user') `  
    -Description:"Company" `  
    -DisplayName:"Company" `  
    -ID:"ad://ext/Company:ContosoAdatum" `  
    -IsSingleValued:$true `  
    -Server:"adatum.com" `  
    -SourceAttribute:Company `  
    -SuggestedValues:@((New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("Contoso", "Contoso", "")), (New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("Adatum", "Adatum", ""))) `  
  
    ```  
  
### <a name="BKMK_2.55"></a>Habilitar la propiedad de recursos de empresa en contoso.com  
  
##### <a name="to-enable-the-company-resource-property-on-contosocom"></a>Para habilitar la propiedad de recursos de empresa en contoso.com  
  
1.  Inicia sesión como administrador en contoso.com.  
  
2.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **centro de administración de Active Directory**.  
  
3.  En el panel izquierdo del centro de administración de Active Directory, haga clic en **vista de árbol**. En el panel izquierdo, haz clic en **Control de acceso dinámico**y, a continuación, haz doble clic en **propiedades de recurso**.  
  
4.  Selecciona **empresa** desde el **propiedades de recurso** lista, haz clic derecho y selecciona **propiedades**. En la **valores sugeridos** sección, haz clic en **agregar** agregar los valores sugeridos: Contoso y Adatum y, a continuación, haz clic en **Aceptar** dos veces.  
  
5.  Selecciona **empresa** desde el **propiedades de recurso** lista, haz clic derecho y selecciona **habilitar**.  
  
### <a name="BKMK_2.6"></a>Habilitar el Control de acceso dinámico en adatum.com  
  
##### <a name="to-enable-dynamic-access-control-for-adatumcom"></a>Para habilitar el Control de acceso dinámico para adatum.com  
  
1.  Inicia sesión como administrador en adatum.com.  
  
2.  Abre la consola de administración de directivas de grupo, haz clic en **adatum.com**y, a continuación, haz doble clic en **controladores de dominio**.  
  
3.  Haz clic en **directiva predeterminada de controladores de dominio**y selecciona **editar**.  
  
4.  En la ventana del Editor de administración de directivas de grupo, haz doble clic en **configuración del equipo**, haz doble clic en **directivas**, haz doble clic en **plantillas administrativas**, haz doble clic en **sistema**y, a continuación, haz doble clic en **KDC**.  
  
5.  Haz doble clic en **compatibilidad de KDC con notificaciones, autenticación compuesta y protección de Kerberos** y selecciona la opción junto a **habilitado**. Debes habilitar esta opción para usar directivas de acceso Central.  
  
6.  Abre un símbolo del sistema con privilegios elevados y ejecuta el siguiente comando:  
  
    ```  
    gpupdate /force  
    ```  
  
### <a name="BKMK_2.8"></a>Crear el tipo de notificación de la empresa en contoso.com  
  
##### <a name="to-create-a-claim-type-by-using-windows-powershell"></a>Para crear un tipo de notificación mediante Windows PowerShell  
  
1.  Inicia sesión como administrador en contoso.com.  
  
2.  Abre un símbolo del sistema con privilegios elevados en Windows PowerShell, escribe el siguiente código:  
  
    ```  
    New-ADClaimType '"SourceTransformPolicy `  
    '"DisplayName 'Company' `  
    '"ID 'ad://ext/Company:ContosoAdatum' `  
    '"IsSingleValued $true `  
    '"ValueType 'string' `  
  
    ```  
  
### <a name="BKMK_2.9"></a>Crear la regla de acceso central  
  
##### <a name="to-create-a-central-access-rule"></a>Para crear una regla de acceso central  
  
1.  En el panel izquierdo del centro de administración de Active Directory, haga clic en **vista de árbol**. En el panel izquierdo, haz clic en **Control de acceso dinámico**y, a continuación, haz clic en **reglas de acceso Central**.  
  
2.  Haz clic en **reglas de acceso Central**, haz clic en **nueva**y luego **reglas de acceso Central**.  
  
3.  En la **nombre** , escriba **AdatumEmployeeAccessRule**.  
  
4.  En la **permisos** sección, selecciona el **usa los siguientes permisos como permisos actuales** opción, haz clic en **editar**y, a continuación, haz clic en **agregar**. Haz clic en el **seleccionar una entidad de seguridad** vincular, escriba **usuarios autenticados**y, a continuación, haz clic en **Aceptar**.  
  
5.  En la **entrada de permiso para permisos** cuadro de diálogo, haz clic en **agregar una condición**y escribe las siguientes condiciones: [**usuario**] [**empresa**] [**es igual a**] [**valor**] [**Adatum**]. Los permisos deben **modificar, leer y ejecutar, leer y escribir**.  
  
6.  Haz clic en **Aceptar**.  
  
7.  Haz clic en **Aceptar** tres veces para finalizar y volver al centro de administración de Active Directory.  
  
    ![guías de solución](media/Appendix-B--Setting-Up-the-Test-Environment/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
    El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.  
  
    ```  
    New-ADCentralAccessRule `  
    -CurrentAcl:"O:SYG:SYD:AR(A;;FA;;;OW)(A;;FA;;;BA)(A;;FA;;;SY)(XA;;0x1301bf;;;AU;(@USER.ad://ext/Company:ContosoAdatum == `"Adatum`"))" `  
    -Name:"AdatumEmployeeAccessRule" `  
    -ProposedAcl:$null `  
    -ProtectedFromAccidentalDeletion:$true `  
    -Server:"contoso.com" `  
    ```  
  
### <a name="BKMK_2.10"></a>Crear la directiva de acceso central  
  
##### <a name="to-create-a-central-access-policy"></a>Para crear una directiva de acceso central  
  
1.  Inicia sesión como administrador en contoso.com.  
  
2.  Abre un símbolo del sistema con privilegios elevados en Windows PowerShell y, a continuación, pega el siguiente código:  
  
    ```  
    New-ADCentralAccessPolicy "Adatum Only Access Policy"   
    Add-ADCentralAccessPolicyMember "Adatum Only Access Policy" `  
    -Member "AdatumEmployeeAccessRule" `  
    ```  
  
### <a name="BKMK_2.11"></a>Publicar la nueva directiva mediante la directiva de grupo  
  
##### <a name="to-apply-the-central-access-policy-across-file-servers-through-group-policy"></a>Para aplicar la directiva de acceso central a través de los servidores de archivos a través de la directiva de grupo  
  
1.  En la **inicio** , escriba **herramientas administrativas**y en la **búsqueda** , haga clic en **configuración**. En la **configuración** resultados, haz clic en **herramientas administrativas**. Abre la consola de administración de directivas de grupo desde la **herramientas administrativas** carpeta.  
  
    > [!TIP]  
    > Si la **herramientas administrativas mostrar** configuración está deshabilitada, la carpeta de herramientas administrativas y su contenido no aparecerá en la **configuración** resultados.  
  
2.  En el dominio contoso.com, haz clic **crear un GPO en este dominio y vincularlo aquí**  
  
3.  Escribe un nombre descriptivo para el GPO, como **AdatumAccessGPO**y, a continuación, haz clic en **Aceptar**.  
  
##### <a name="to-apply-the-central-access-policy-to-the-file-server-through-group-policy"></a>Para aplicar la directiva de acceso central al servidor de archivos mediante la directiva de grupo  
  
1.  En la **inicio** , escriba **Group Policy Management**, en la **búsqueda** cuadro. Abre **Group Policy Management** desde la carpeta de herramientas administrativas.  
  
    > [!TIP]  
    > Si la **herramientas administrativas mostrar** configuración está deshabilitada, la carpeta de herramientas administrativas y su contenido no aparecerá en los resultados de la configuración.  
  
2.  Navega hasta y selecciona Contoso como sigue: Management\Forest de directiva de grupo: contoso.com\Domains\contoso.com.  
  
3.  Haz clic en el **AdatumAccessGPO** directiva y selecciona **editar**.  
  
4.  En el Editor de administración de directivas de grupo, haz clic en **configuración del equipo**, expanda **directivas**, expanda **configuración de Windows**y, a continuación, haz clic en **la configuración de seguridad**.  
  
5.  Expande **sistema de archivos**, haz clic en **directiva de acceso Central**y, a continuación, haz clic en **directivas de acceso Central de administrar**.  
  
6.  En la **configuración de directivas de acceso Central** cuadro de diálogo, haz clic en **agregar**, selecciona **Adatum solo la directiva de acceso**y, a continuación, haz clic en **Aceptar**.  
  
7.  Cierra el Editor de administración de directivas de grupo. Ahora que hayas agregado la directiva de acceso central en la directiva de grupo.  
  
### <a name="BKMK_2.12"></a>Crear la carpeta de ganancias en el servidor de archivos  
Crear un nuevo volumen NTFS en archivo1 y crea la siguiente carpeta: D:\Earnings.  
  
> [!NOTE]  
> Directivas de acceso central no están habilitadas de manera predeterminada en el sistema o el volumen C: de arranque.  
  
### <a name="BKMK_2.13"></a>Establecer clasificación y aplica la directiva de acceso central en la carpeta de ganancias  
  
##### <a name="to-assign-the-central-access-policy-on-the-file-server"></a>Para asignar la directiva de acceso central en el servidor de archivos  
  
1.  En el Administrador de Hyper-V, conecta al servidor archivo1. Iniciar sesión el servidor mediante Contoso\Administrator, con la contraseña ** pass@word1 **.  
  
2.  Abre un símbolo del sistema con privilegios elevados y escribe: **comando gpupdate/force**. Esto garantiza que los cambios de directiva de grupo surtirán efecto en el servidor.  
  
3.  También deberás actualizar las propiedades del recurso Global de Active Directory. Abre Windows PowerShell, escribe `Update-FSRMClassificationpropertyDefinition`, y, a continuación, presione ENTRAR. Cierra Windows PowerShell.  
  
4.  Abre el Explorador de Windows y navegar a D:\EARNINGS. Haz clic en el **ganancias** carpeta y haz clic en **propiedades**.  
  
5.  Haz clic en el **clasificación** ficha **empresa**y, a continuación, selecciona **Adatum** en la **valor** campo.  
  
6.  Haz clic en **cambio**, selecciona **Adatum solo la directiva de acceso** desde el menú desplegable y, a continuación, haz clic en **aplicar**.  
  
7.  Haz clic en el **seguridad**, haga clic **avanzadas**y, a continuación, haz clic en el **directiva Central** pestaña. Deberías ver el **AdatumEmployeeAccessRule** enumerados. Puedes expandir el elemento para ver todos los permisos que se establecen al crear la regla en Active Directory.  
  
8.  Haz clic en **Aceptar** para volver a Windows Explorer.  
  



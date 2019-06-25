---
ms.assetid: 82918181-525d-4e93-af96-957dac6aedb6
title: 'Apéndice B: configuración del entorno de pruebas'
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3ebe125ce7850797d786e7b564c98889cfb19927
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445862"
---
# <a name="appendix-b-setting-up-the-test-environment"></a>Apéndice B: Configuración del entorno de pruebas

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se describen los pasos para crear un laboratorio práctico para probar el control de acceso dinámico. Las instrucciones deben seguirse secuencialmente porque hay muchos componentes que tienen dependencias.  

## <a name="prerequisites"></a>Requisitos previos  
**Requisitos de hardware y software**  

Requisitos para configurar el laboratorio de pruebas:  

-   Un servidor host con Windows Server 2008 R2 con SP1 e Hyper-V  

-   Una copia de la imagen ISO de Windows Server 2012  

-   Una copia de la imagen de ISO de Windows 8  

-   Microsoft Office 2010  

-   Un servidor con Microsoft Exchange Server 2003 o posterior  

Necesitas crear las siguientes máquinas virtuales para probar los escenarios de control de acceso dinámico:  

-   DC1 (controlador de dominio)  

-   DC2 (controlador de dominio)  

-   FILE1 (servidor de archivos y Active Directory Rights Management Services)  

-   SRV1 (servidores POP3 y SMTP)  

-   CLIENT1 (equipo cliente con Microsoft Outlook)  

Las contraseñas para las máquinas virtuales deben ser las siguientes:  

-   BUILTIN\Administrator: pass@word1  

-   CONTOSO\Administrador: pass@word1  

-   Todas las demás cuentas: pass@word1  

## <a name="build-the-test-lab-virtual-machines"></a>Crear las máquinas virtuales del laboratorio de pruebas  

### <a name="install-the-hyper-v-role"></a>Instalar el rol Hyper-V  
Tienes que instalar el rol Hyper-V en un equipo con Windows Server 2008 R2 con SP1.  

##### <a name="to-install-the-hyper-v-role"></a>Para instalar el rol Hyper-V  

1.  Haga clic en **Inicio**y, después, en Administrador del servidor.  

2.  En el área de resumen de los roles de la ventana principal del Administrador del servidor, haz clic en **Agregar roles**.  

3.  En la pantalla **Seleccionar roles de servidor**, selecciona **Hyper-V**.  

4.  En la página **Crear redes virtuales** , haz clic en uno o varios adaptadores si quieres que sus conexiones de red estén disponibles para las máquinas virtuales.  

5.  En la página para **confirmar selecciones de instalación**, haga clic en **Instalar**.  

6.  El equipo debe reiniciarse para finalizar la instalación. Haz clic en **Cerrar** para finalizar el asistente y después haz clic en **Sí** para reiniciar el servidor.  

7.  Después de reiniciar el servidor, inicia sesión con la misma cuenta que usaste para instalar el rol. Cuando el Asistente para reanudar la configuración complete la instalación, haz clic en **Cerrar** para finalizar el asistente.  

### <a name="create-an-internal-virtual-network"></a>Crear una red virtual interna  
Ahora crearás una red virtual interna llamada ID_AD_Network.  

##### <a name="to-create-a-virtual-network"></a>Para crear una red virtual  

1.  Abre el Administrador Hyper-V.  

2.  En el menú **Acciones**, haz clic en **Administrador de redes virtuales**.  

3.  En **Crear red virtual**, selecciona **Interna**.  

4.  Haz clic en **Agregar**. Se abre la página **Nueva red virtual** .  

5.  Escribe **ID_AD_Network** como nombre de la nueva red. Revisa las demás propiedades y modifícalas si es necesario.  

6.  Haz clic en **Aceptar** para crear la red virtual y cerrar el Administrador de redes virtuales o haz clic en **Aplicar** para crear la red virtual y continuar usando el Administrador de redes virtuales.  

### <a name="BKMK_Build"></a>Crear el controlador de dominio  
Crea una máquina virtual para usarla como controlador de dominio (DC1). Instale la máquina virtual con Windows Server 2012 ISO y asígnele el nombre DC1.  

##### <a name="to-install-active-directory-domain-services"></a>Para instalar los Servicios de dominio de Active Directory  

1. Conecta la máquina virtual a ID_AD_Network. Inicie sesión DC1 como administrador con la contraseña <strong>pass@word1</strong>.  

2. En el Administrador del servidor, haga clic en **Administrar** y en **Agregar roles y características**.  

3. En la página **Antes de comenzar** , haga clic en **Siguiente**.  

4. En la página **Seleccionar tipo de instalación** , haz clic en **Instalación basada en características o en roles**y, después, haz clic en **Siguiente**.  

5. En la página **Seleccionar servidor de destino**, haga clic en **Siguiente**.  

6. En la página **Seleccionar roles de servidor**, haz clic en **Servicios de federación de Active Directory**. En el cuadro de diálogo **Asistente para agregar roles y características**, haz clic en **Agregar características** y, después, en **Siguiente**.  

7. En la página **Seleccionar características**, haga clic en **Siguiente**.  

8. En la página **Servicios de dominio de Active Directory** , revisa la información y haz clic en **Siguiente**.  

9. En la página **Confirmar selecciones de instalación** , haga clic en **Instalar**. La barra de progreso de instalación de características en la página Resultados indica que el rol se está instalando.  

10. En la página **Resultados**, comprueba que la instalación se realizó correctamente y haz clic en **Cerrar**. En el Administrador del servidor, haz clic en el icono de advertencia con una marca de exclamación, en la esquina superior derecha de la pantalla, junto a **Administrar**. En la lista Tareas, haz clic en el vínculo **Promover este servidor a controlador de dominio**.  

11. En la página **Configuración de implementación**, haz clic en **Agregar un nuevo bosque**, escribe el nombre del dominio raíz, **contoso.com** y haz clic en **Siguiente**.  

12. En el **opciones del controlador de dominio** página, seleccione los niveles funcionales de dominio y bosque como Windows Server 2012, especifique la contraseña de DSRM <strong>pass@word1</strong>y, a continuación, haga clic en **siguiente**.  

13. En la página **Opciones de DNS**, haz clic en **Siguiente**.  

14. En la página **Opciones adicionales** , haz clic en **Siguiente**.  

15. En la página **Rutas de acceso** , escribe las ubicaciones para la carpeta SYSVOL, archivos de registro o la base de datos de Active Directory (o acepta las ubicaciones predeterminadas) y, después, haz clic en **Siguiente**.  

16. En la página **Revisar opciones** , confirma las selecciones y, después, haz clic en **Siguiente**.  

17. En la página **Comprobación de requisitos previos**, confirma que se haya completado la validación de los requisitos previos y, después, haz clic en **Instalar**.  

18. En la página **Resultados**, comprueba que el servidor se haya configurado correctamente como controlador de dominio y haz clic en **Cerrar**.  

19. Reinicia el servidor para completar la instalación de AD DS. (De forma predeterminada, esto se produce automáticamente).  

Crea los siguientes usuarios con el Centro de administración de Active Directory.  

##### <a name="create-users-and-groups-on-dc1"></a>Crear usuarios y grupos en DC1  

1. Inicia sesión en contoso.com como Administrador. Abre el Centro de administración de Active Directory.  

2. Crea los siguientes grupos de seguridad:  


   |    Nombre de grupo    |        Dirección de correo electrónico         |
   |------------------|------------------------------|
   |   FinanceAdmin   |   financeadmin@contoso.com   |
   | FinanceException | financeexception@contoso.com |


3. Crea la siguiente unidad organizativa (OU):  


   |   Nombre de OU    | Equipos |
   |--------------|-----------|
   | FileServerOU |   FILE1   |


4. Crea los siguientes usuarios con los atributos indicados:  


   |       Usuario       |  Nombre de usuario  |     Dirección de correo electrónico      | Departmento |      Agrupar       | País o región |
   |------------------|------------|------------------------|------------|------------------|----------------|
   | Myriam Delesalle | MDelesalle | MDelesalle@contoso.com |  Finanzas   |                  |       EE. UU.       |
   |    Miles Reid    |   MReid    |   MReid@contoso.com    |  Finanzas   |   FinanceAdmin   |       EE. UU.       |
   |   Esther Valle   |   EValle   |   EValle@contoso.com   | Operaciones | FinanceException |       EE. UU.       |
   |   Maira Wenzel   |  MWenzel   |  MWenzel@contoso.com   |     HR     |                  |       EE. UU.       |
   |     Jeff Low     |    JLow    |    JLow@contoso.com    |     HR     |                  |       EE. UU.       |
   |    Servidor RMS    |    rms     |    rms@contoso.com     |            |                  |                |

   Para obtener más información sobre cómo crear grupos de seguridad, consulte [Crear un nuevo grupo](https://technet.microsoft.com/library/dd861305.aspx) en el sitio web de Windows Server.  

##### <a name="to-create-a-group-policy-object"></a>Para crear un objeto de directiva de grupo  

1.  Mantén el cursor en la esquina superior derecha de la pantalla y haz clic en el icono de búsqueda. En el cuadro de búsqueda, escribe **administración de directivas de grupo** y haz clic en **Administración de directivas de grupo**.  

2.  Expande **Bosque: contoso.com**y, después, expande **Dominios**, navega a **contoso.com**, expande **(contoso.com)** y selecciona **FileServerOU**. Haga clic en **crear un GPO en este dominio y vincularlo aquí**

3.  Escribe un nombre descriptivo para el GPO, como **FlexibleAccessGPO**, y haz clic en **Aceptar**.  

##### <a name="to-enable-dynamic-access-control-for-contosocom"></a>Para habilitar el control de acceso dinámico para contoso.com  

1.  Abre la Consola de administración de directivas de grupo, haz clic en **contoso.com** y, después, haz doble clic en **Controladores de dominio**.  

2.  Haz clic con el botón derecho en **Directiva predeterminada de controladores de dominio**y selecciona **Editar**.  

3.  En la ventana del Editor de administración de directivas de grupo, haz doble clic en **Configuración del equipo**, haz doble clic en **Directivas**, haz doble clic en **Plantillas administrativas**, haz doble clic en **Sistema**y, después, haz doble clic en **KDC**.  

4.  Haz doble clic en **Compatibilidad del KDC con notificaciones, autenticación compuesta y protección de Kerberos** y selecciona la opción junto a **Habilitado**. Necesitas habilitar esta opción para usar las directivas de acceso central.  

5.  Abre un símbolo del sistema con permisos elevados y ejecuta el siguiente comando:  

    ```  
    gpupdate /force  
    ```  

### <a name="BKMK_FS1"></a>Crear el servidor de archivos y el servidor de AD RMS (FILE1)  

1. Construido a una máquina virtual con el nombre FILE1 a partir de la imagen ISO de Windows Server 2012.  

2. Conecta la máquina virtual a ID_AD_Network.  

3. Unir la máquina virtual al dominio contoso.com y, a continuación, inicie sesión en FILE1 como Contoso\Administrador con la contraseña <strong>pass@word1</strong>.  

#### <a name="install-file-services-resource-manager"></a>Instalar el Administrador de recursos del servidor de archivos  

###### <a name="to-install-the-file-services-role-and-the-file-server-resource-manager"></a>Para instalar el rol Servicios de archivo y el Administrador de recursos del servidor de archivos  

1.  En el Administrador del servidor, haz clic en **Agregar roles y características**.  

2.  En la página **Antes de comenzar** , haga clic en **Siguiente**.  

3.  En la página **Seleccionar tipo de instalación** , haz clic en **Siguiente**.  

4.  En la página **Seleccionar servidor de destino**, haga clic en **Siguiente**.  

5.  En la página **Seleccionar roles de servidor** , expande **Servicios de archivos y almacenamiento**, activa la casilla junto a **Servicios de iSCSI y archivo**, expande y selecciona **Administrador de recursos del servidor de archivos**.  

    En el Asistente para agregar roles y características, haz clic en **Agregar características**y, después, en **Siguiente**.  

6.  En la página **Seleccionar características**, haga clic en **Siguiente**.  

7.  En la página **Confirmar selecciones de instalación** , haga clic en **Instalar**.  

8.  En la página **Progreso de la instalación**, haz clic en **Cerrar**.  

#### <a name="install-the-microsoft-office-filter-packs-on-the-file-server"></a>Instalar los paquetes de filtros de Microsoft Office en el servidor de archivos  
Debe instalar los paquetes de filtros de Microsoft Office en Windows Server 2012 para habilitar IFilters para una variedad de archivos de Office que se proporcionan de forma predeterminada.  Windows Server 2012 no tiene los IFilters para archivos de Microsoft Office instala de forma predeterminada, y la infraestructura de clasificación de archivos usa IFilters para realizar análisis de contenido.  

Para descargar e instalar los IFilters, consulte [Paquetes de filtros de Microsoft Office 2010](https://go.microsoft.com/fwlink/?LinkID=234122).  

#### <a name="configure-email-notifications-on-file1"></a>Configurar notificaciones de correo electrónico en FILE1  
Al crear cuotas y filtros de archivos, tienes la opción de enviar notificaciones de correo electrónico a los usuarios cuando estén llegando al límite de su cuota o cuando hayan intentado guardar archivos que han sido bloqueados. Si quieres notificar de forma rutinaria a determinados administradores los eventos de cuota y filtrado de archivos, puedes configurar uno o varios destinatarios predeterminados. Para enviar estas notificaciones, debes especificar el servidor SMTP que se utilizará para reenviar los mensajes de correo electrónico.  

###### <a name="to-configure-email-options-in-file-server-resource-manager"></a>Para configurar las opciones de correo electrónico en el Administrador de recursos del servidor de archivos  

1. Abra el Administrador de recursos del servidor de archivos. Para abrir el Administrador de recursos del servidor de archivos haz clic en **Inicio**, escribe **administrador de recursos del servidor de archivos**y haz clic en **Administrador de recursos del servidor de archivos**.  

2. En la interfaz del Administrador de recursos del servidor de archivos, haz clic con el botón derecho en **Administrador de recursos del servidor de archivos** y, después, haz clic en **Configurar opciones**. Se abre el cuadro de diálogo **Opciones del Administrador de recursos del servidor de archivos**.  

3. En la pestaña **Notificaciones de correo electrónico** , en el nombre o la dirección IP del servidor SMTP, escribe el nombre de host o la dirección IP del servidor SMTP que reenviará las notificaciones de correo electrónico.  

4. Si desea notificar a determinados administradores de cuota habitualmente o eventos de filtrado, de archivos en **Administradores receptores predeterminados**, escriba cada dirección de correo electrónico como fileadmin@contoso.com. Use el formato account@domainy use punto y coma para separar varias cuentas.  

#### <a name="create-groups-on-file1"></a>Crear grupos en FILE1  

###### <a name="to-create-security-groups-on-file1"></a>Para crear grupos de seguridad en FILE1  

1. Inicie sesión en FILE1 como Contoso\Administrador con la contraseña: <strong>pass@word1</strong>.  

2. Agrega NT AUTHORITY\Usuarios autenticados al grupo **WinRMRemoteWMIUsers__** .  

#### <a name="create-files-and-folders-on-file1"></a>Crear archivos y carpetas en FILE1  

1.  Crea un nuevo volumen NTFS en FILE1 y, después, crea la siguiente carpeta: D:\Finance Documents.  

2.  Crea los siguientes archivos con los detalles especificados:  

    -   **Finance Memo.docx**: Agrega texto financiero al documento. Por ejemplo, "las reglas empresariales sobre quién puede tener acceso a los documentos financieros han cambiado. Ahora, solo los miembros del grupo FinanceExpert pueden acceder a los documentos financieros. No hay otros departamentos o grupos tienen acceso. " Tienes que evaluar el impacto de este cambio antes de implementarlo en el entorno. Asegúrate de que en el pie de página de todas las páginas de este documento aparece CONTOSO CONFIDENTIAL.  

    -   **Request for Approval to Hire.docx**: Crea un formulario en este documento que recopile la información del candidato. El documento debe tener los siguientes campos: **Applicant Name, Social Security number, Job Title, Proposed Salary, Starting Date, Supervisor name, Department** (Nombre del candidato, N.º de la Seguridad Social, Puesto, Salario propuesto, Fecha de inicio, Nombre del supervisor, Departamento). Agrega una sección adicional al documento que tenga un formulario con **Supervisor Signature, Approved Salary, Conformation of Offer** y **Status of Offer** (Firma del supervisor, Salario aprobado, Detalle de la oferta y Estado de la oferta).   
        Habilite la administración de derechos del documento.  

    -   **Word Document1.docx**: Agrega contenido de prueba a este documento.  

    -   **Word Document2.docx**: Agrega contenido de prueba a este documento.  

    -   **Workbook1.xlsx**  

    -   **Workbook2.xlsx**  

    -   Crea una carpeta en el escritorio que se llame Expresiones regulares. Crea un documento de texto en la carpeta **RegEx-SSN**. Escribe el siguiente contenido en el archivo y, después, guárdalo y ciérralo:   
        ^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$  

3.  Comparte la carpeta D:\Finance Documents como Finance Documents y permite que todos tengan acceso de lectura y escritura al recurso compartido.  

> [!NOTE]  
> Las directivas de acceso central no están habilitadas de forma predeterminada en el volumen C: de arranque o del sistema.  

#### <a name="BKMK_CS1"></a>Instalar Active Directory Rights Management Services  
Agrega Active Directory Rights Management Services (AD RMS) y todas las características necesarias a través del Administrador del servidor. Selecciona todos los valores predeterminados.  

###### <a name="to-install-active-directory-rights-management-services"></a>Para instalar Active Directory Rights Management Services  

1. Inicia sesión en FILE1 como CONTOSO\Administrador o como miembro del grupo Admins. del dominio.  

   > [!IMPORTANT]  
   > Para instalar el rol de servidor de AD RMS, la cuenta del instalador (en este caso, CONTOSO\Administrador) tendrá que pertenecer al grupo local Administradores en el equipo servidor donde se va a instalar AD RMS y al grupo Administradores de organización en Active Directory.  

2. En el Administrador del servidor, haz clic en **Agregar roles y características**. Aparece el Asistente para agregar roles y características.  

3. En la pantalla **Antes de comenzar**, haz clic en **Siguiente**.  

4. En la pantalla **Seleccionar tipo de instalación** , haz clic en **Instalación basada en características o en roles**y, después, haz clic en **Siguiente**.  

5. En la pantalla **Seleccionar destinos del servidor**, haz clic en **Siguiente**.  

6. En la pantalla **Seleccionar roles de servidor** , activa la casilla junto a **Active Directory Rights Management Services**y, después, haz clic en **Siguiente**.  

7. En **¿Desea agregar características requeridas para Active Directory Rights Management Services?** , haz clic en **Agregar características**.  

8. En la pantalla **Seleccionar roles de servidor**, haz clic en **Siguiente**.  

9. En la pantalla **Seleccionar características para instalar** , haz clic en **Siguiente**.  

10. En la pantalla **Active Directory Rights Management Services**, haz clic en Siguiente.  

11. En la pantalla **Seleccionar servicios de rol** , haz clic en **Siguiente**.  

12. En la pantalla **Rol Servidor web (IIS)** , haga clic en **Siguiente**.  

13. En la pantalla **Seleccionar servicios de rol** , haz clic en **Siguiente**.  

14. En la pantalla **Confirmar selecciones de instalación** , haz clic en **Instalar**.  

15. Una vez completada la instalación, en la pantalla **Progreso de la instalación** , haz clic en **Realizar configuración adicional**. Aparece el asistente de configuración de AD RMS.  

16. En la pantalla **AD RMS** , haz clic en **Siguiente**.  

17. En la pantalla **Clúster de AD RMS** , selecciona **Crear un nuevo clúster raíz de AD RMS** y, después, haz clic en **Siguiente**.  

18. En la pantalla **Base de datos de configuración**, haz clic en **Usar Windows Internal Database en este servidor** y, después, haz clic en **Siguiente**.  

    > [!NOTE]  
    > Se recomienda usar Windows Internal Database solo en entornos de prueba porque no admite más de un servidor en el clúster de AD RMS. Las implementaciones de producción usan un servidor de base de datos diferente.  

19. En el **cuenta de servicio** pantalla, en **cuenta de usuario de dominio**, haga clic en **especifique** y, a continuación, especifique el nombre de usuario (**contoso\rms**), y Contraseña (<strong>pass@word1</strong>) y haga clic en **Aceptar**y, a continuación, haga clic en **siguiente**.  

20. En la pantalla **Modo criptográfico**, haz clic en **Modo criptográfico 2**.  

21. En la pantalla **Almacenamiento de la clave de clúster**, haz clic en **Siguiente**.  

22. En el **contraseña de la clave de clúster** pantalla, en el **contraseña** y **Confirmar contraseña** cuadros, escriba <strong>pass@word1</strong>y, a continuación, haga clic en **Siguiente**.  

23. En la pantalla **Sitio web del clúster** , asegúrate de que **Sitio web predeterminado** está seleccionado y, después, haz clic en **Siguiente**.  

24. En la pantalla **Dirección de clúster**, selecciona la opción **Usar una conexión no cifrada**, en el cuadro **Nombre de domino completo**, escribe **FILE1.contoso.com** y, después, haz clic en **Siguiente**.  

25. En la pantalla **Nombre del certificado emisor de licencias**, acepta el nombre predeterminado (**FILE1**) en el cuadro de texto y haz clic en **Siguiente**.  

26. En la pantalla **Registro de SCP** , selecciona **Registrar el SCP ahora**y, después, haz clic en **Siguiente**.  

27. En la pantalla **Confirmación**, haz clic en **Instalar**.  

28. En la pantalla **Resultados**, haz clic en **Cerrar** y, después, haz clic en **Cerrar** en la pantalla **Progreso de la instalación**. Cuando haya terminado, cierre la sesión e iniciar sesión como contoso\rms usando la contraseña proporcionada (<strong>pass@word1</strong>).  

29. Inicia la consola de AD RMS y ve a **Plantillas de directiva de permisos**.  

    Para abrir la consola de AD RMS, en el Administrador del servidor, haz clic en **Servidor local** en el árbol de consola, haz clic en **Herramientas**y haz clic en **Active Directory Rights Management Services**.  

30. Haz clic en la plantilla **Crear plantilla de directiva de permisos distribuida** , situada en el panel derecho, haz clic en **Agregar**y selecciona la siguiente información:  

    -   Language (idioma): Inglés EE.UU.  

    -   Nombre: Contoso Finance Admin Only  

    -   Descripción: Contoso Finance Admin Only  

    Haga clic en **Agregar** y, a continuación, en **Siguiente**.  

31. En la sección usuarios y derechos, haga clic en **usuarios y derechos**, haga clic en **agregar**, tipo <strong>financeadmin@contoso.com</strong>y haga clic en **Aceptar**.  

32. Selecciona **Control total**y deja seleccionada **Conceder al propietario (autor) derecho de control total sin expiración** .  

33. Haz clic en las demás pestañas sin realizar cambios y, después, haz clic en **Finalizar**. Inicie sesión como CONTOSO\Administrador.  

34. Vaya a la carpeta C:\inetpub\wwwroot\\_wmcs\certification, seleccione el archivo ServerCertification.asmx y agrega usuarios autenticados para leído y permisos de escritura en el archivo.  

35. Abra Windows PowerShell y ejecute `Get-FsrmRmsTemplate`. Comprueba que puedes ver la plantilla RMS que creaste en los pasos anteriores de este procedimiento, con este comando.  

> [!IMPORTANT]  
> Si quieres que tus servidores de archivos cambien inmediatamente para poder probarlos, tienes que hacer lo siguiente:  
>   
> 1.  En el servidor de archivos FILE1, abre un símbolo del sistema con privilegios elevados y ejecuta los siguientes comandos:  
>   
>     -   gpupdate /force.  
>     -   NLTEST /SC_RESET:contoso.com  
> 2.  En el controlador de dominio (DC1), replica Active Directory.  
>   
>     Para obtener más información acerca de los pasos para aplicar la replicación de Active Directory, consulte [Replicación de Active Directory](https://technet.microsoft.com/library/cc794809(WS.10).aspx)  

En lugar de usar el Asistente para agregar roles y características en el Administrador del servidor, también puedes usar Windows PowerShell para instalar y configurar el rol de servidor de AD RMS tal y como se muestra en el procedimiento siguiente.  

###### <a name="to-install-and-configure-an-ad-rms-cluster-in-windows-server-2012-using-windows-powershell"></a>Para instalar y configurar un clúster de AD RMS en Windows Server 2012 mediante Windows PowerShell  

1. Inicie sesión como Contoso\Administrador con la contraseña: <strong>pass@word1</strong>.  

   > [!IMPORTANT]  
   > Para instalar el rol de servidor de AD RMS, la cuenta del instalador (en este caso, CONTOSO\Administrador) tendrá que pertenecer al grupo local Administradores en el equipo servidor donde se va a instalar AD RMS y al grupo Administradores de organización en Active Directory.  

2. En el escritorio del servidor, haz clic con el botón derecho en el icono de Windows PowerShell en la barra de tareas, y selecciona **Ejecutar como administrador** para abrir un símbolo del sistema de Windows PowerShell con privilegios administrativos.  

3. Para usar los cmdlets del Administrador del servidor para instalar el rol de servidor de AD RMS, escribe:  

   ```  
   Add-WindowsFeature ADRMS '"IncludeAllSubFeature '"IncludeManagementTools  
   ```  

4. Crea la unidad de Windows PowerShell para que represente el servidor de AD RMS que vas a instalar.  

   Por ejemplo, para crear una unidad de Windows PowerShell llamada RC para instalar y configurar el primer servidor de un clúster raíz de AD RMS, escribe:  

   ```  
   Import-Module ADRMS  
   New-PSDrive -PSProvider ADRMSInstall -Name RC -Root RootCluster  
   ```  

5. En los objetos del espacio de nombres de la unidad, establece las propiedades que representan las opciones de configuración necesarias.  

   Por ejemplo, para establecer la cuenta de servicio AD RMS, en el símbolo del sistema de Windows PowerShell, escribe:  

   ```  
   $svcacct = Get-Credential  
   ```  

   Cuando aparezca el cuadro de diálogo de seguridad de Windows, escribe el nombre de usuario de la cuenta de servicio de AD RMS CONTOSO\RMS y la contraseña asignada.  

   Después, para asignar la cuenta de servicio de AD RMS a la configuración del clúster de AD RMS, escribe lo siguiente:  

   ```  
   Set-ItemProperty -Path RC:\ -Name ServiceAccount -Value $svcacct  
   ```  

   Después, para establecer el servidor de AD RMS para que use Windows Internal Database, en el símbolo del sistema de Windows PowerShell, escribe:  

   ```  
   Set-ItemProperty -Path RC:\ClusterDatabase -Name UseWindowsInternalDatabase -Value $true  
   ```  

   Después, para almacenar la contraseña de la clave de clúster de forma segura en una variable, en el símbolo del sistema de Windows PowerShell, escribe:  

   ```  
   $password = Read-Host -AsSecureString -Prompt "Password:"  
   ```  

   Escribe la contraseña de la clave de clúster y presiona ENTRAR.  

   Después, para asignar la contraseña a tu instalación de AD RMS, en el símbolo del sistema de Windows PowerShell, escribe:  

   ```  
   Set-ItemProperty -Path RC:\ClusterKey -Name CentrallyManagedPassword -Value $password  
   ```  

   Después, para establecer la dirección del clúster de AD RMS, en el símbolo del sistema de Windows PowerShell, escribe:  

   ```  
   Set-ItemProperty -Path RC:\ -Name ClusterURL -Value "http://file1.contoso.com:80"  
   ```  

   Después, para asignar el nombre de SLC a tu instalación de AD RMS, en el símbolo del sistema de Windows PowerShell, escribe:  

   ```  
   Set-ItemProperty -Path RC:\ -Name SLCName -Value "FILE1"  
   ```  

   Después, para establecer el punto de conexión de servicio (SCP) al clúster de AD RMS, en el símbolo del sistema de Windows PowerShell, escribe:  

   ```  
   Set-ItemProperty -Path RC:\ -Name RegisterSCP -Value $true  
   ```  

6. Ejecuta el cmdlet **Install-ADRMS**. Además de instalar el rol de servidor de AD RMS y de configurar el servidor, este cmdlet también instala otras características que AD RMS requiere, si es necesario.  

   Por ejemplo, para cambiar a la unidad de Windows PowerShell llamada RC e instalar y configurar AD RMS, escribe:  

   ```  
   Set-Location RC:\  
   Install-ADRMS -Path.  
   ```  

   Escribe "S" cuando el cmdlet te pida que confirmes si quieres iniciar la instalación.  

7. Cerrar sesión como Contoso\Administrador y de registro en nuevo como CONTOSO\RMS usando la contraseña proporcionada ("pass@word1").  

   > [!IMPORTANT]  
   > Para administrar el servidor de AD RMS, la cuenta con la que has iniciado sesión y que estás usando para administrar el servidor (en este caso, CONTOSO\RMS) tendrá que pertenecer al grupo local Administradores en el equipo servidor de AD RMS y al grupo Administradores de organización en Active Directory.  

8. En el escritorio del servidor, haz clic con el botón derecho en el icono de Windows PowerShell en la barra de tareas, y selecciona **Ejecutar como administrador** para abrir un símbolo del sistema de Windows PowerShell con privilegios administrativos.  

9. Crea la unidad de Windows PowerShell para que represente el servidor de AD RMS que vas a configurar.  

    Por ejemplo, para crear una unidad de Windows PowerShell llamada RC para configurar el clúster raíz de AD RMS, escribe:  

    ```  
    Import-Module ADRMSAdmin `  
    New-PSDrive -PSProvider ADRMSAdmin -Name RC -Root http://localhost -Force -Scope Global  
    ```  

10. Para crear nuevas plantillas de derechos para el administrador financiero de Contoso y asignarle derechos de usuario con control total en tu instalación de AD RMS, en el símbolo del sistema de Windows PowerShell, escribe:  

    ```  
    New-Item -Path RC:\RightsPolicyTemplate '"LocaleName en-us -DisplayName "Contoso Finance Admin Only" -Description "Contoso Finance Admin Only" -UserGroup financeadmin@contoso.com  -Right ('FullControl')  
    ```  

11. Para comprobar que puedes ver la nueva plantilla de derechos para el administrador financiero de Contoso, en el símbolo del sistema de Windows PowerShell:  

    ```  
    Get-FsrmRmsTemplate  
    ```  

    Revisa el resultado de este cmdlet para confirmar que la plantilla RMS que creaste en el paso anterior está presente.  

### <a name="build-the-mail-server-srv1"></a>Crea el servidor de correo (SRV1)  
SRV1 es el servidor de correo SMTP/POP3. Tienes que configurarlo para poder enviar notificaciones de correo electrónico como parte del escenario de asistencia de acceso denegado.  

Configura Microsoft Exchange Server en este equipo. Para obtener más información, consulte [Cómo instalar Exchange Server](https://go.microsoft.com/fwlink/?prd=12364).  

### <a name="build-the-client-virtual-machine-client1"></a>Crea la máquina virtual cliente (CLIENT1)  

##### <a name="to-build-the-client-virtual-machine"></a>Para crear la máquina virtual cliente  

1. Conecta CLIENT1 a ID_AD_Network.  

2. Instala Microsoft Office 2010.  

3. Inicia sesión como Contoso\Administrador y usa la siguiente información para configurar Microsoft Outlook.  

   - Tu nombre: Administrador de archivos  

   - Dirección de correo electrónico: fileadmin@contoso.com  

   - Tipo de cuenta: POP3  

   - Servidor de correo entrante: Dirección IP estática de SRV1  

   - Servidor de correo saliente: Dirección IP estática de SRV1  

   - Nombre de usuario: fileadmin@contoso.com  

   - Recordar contraseña: Seleccionar  

4. Crea un acceso directo a Outlook en el escritorio de contoso\administrador.  

5. Abre Outlook y consulta todos los mensajes de "primer inicio".  

6. Elimina los mensajes de prueba que se generaron.  

7. Crear un nuevo acceso directo en el escritorio para todos los usuarios en la máquina virtual de cliente que apunta a \\\FILE1\Finance documentos.  

8. Reinicia cuando sea necesario.  

##### <a name="enable-access-denied-assistance-on-the-client-virtual-machine"></a>Habilita la asistencia de acceso denegado en la máquina virtual cliente  

1.  Abre el Editor del Registro y ve a **HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Explorer**.  

    -   Establece **EnableShellExecuteFileStreamCheck** en **1**.  

    -   Valor: DWORD  

## <a name="BKMK_CF"></a>Configuración del laboratorio para la implementación de notificaciones en el escenario de bosques  

### <a name="BKMK_2.1"></a>Crear una máquina virtual para DC2  

-   Crear una máquina virtual desde la imagen ISO de Windows Server 2012.  

-   Asigna el nombre DC2 a la máquina virtual.  

-   Conecta la máquina virtual a ID_AD_Network.  

> [!IMPORTANT]  
> Para unir máquinas virtuales a un dominio e implementar tipos de notificaciones en bosques es necesario que las máquinas virtuales puedan resolver los nombres completos de los dominios relevantes. Para ello, puede que tengas que configurar manualmente las opciones de DNS en las máquinas virtuales. Para obtener más información, consulte [Configuración de una red virtual](https://technet.microsoft.com/library/cc732470%28v=ws.10%29.aspx).  
>   
> Todas las imágenes de máquinas virtuales (servidores y clientes) deben reconfigurarse para que usen una dirección IP estática de la versión 4 (IPv4) y la configuración de cliente del Sistema de nombres de dominio (DNS). Para obtener más información, consulte [Configurar un cliente DNS con una dirección IP estática](https://go.microsoft.com/fwlink/?LinkId=150952).  

### <a name="BKMK_2.2"></a>Configurar un nuevo bosque llamado adatum.com  

##### <a name="to-install-active-directory-domain-services"></a>Para instalar los Servicios de dominio de Active Directory  

1. Conecta la máquina virtual a ID_AD_Network. Inicie sesión DC2 como administrador con la contraseña <strong>Pass@word1</strong>.  

2. En el Administrador del servidor, haga clic en **Administrar** y en **Agregar roles y características**.  

3. En la página **Antes de comenzar** , haga clic en **Siguiente**.  

4. En la página **Seleccionar tipo de instalación** , haz clic en **Instalación basada en características o en roles**y, después, haz clic en **Siguiente**.  

5. En la página **Seleccionar servidor de destino** , haz clic en **Seleccionar un servidor del grupo de servidores**, haz clic en los nombres de servidor donde quieres instalar los Servicios de dominio de Active Directory (AD DS) y, después, en **Siguiente**.  

6. En la página **Seleccionar roles de servidor** , haz clic en **Servicios de dominio de Active Directory**. En el cuadro de diálogo **Asistente para agregar roles y características**, haz clic en **Agregar características** y, después, en **Siguiente**.  

7. En la página **Seleccionar características**, haz clic en **Siguiente**.  

8. En la página **AD DS**, revisa la información y, después, haz clic en **Siguiente**.  

9. En la página **Confirmación**, haz clic en **Instalar**. La barra de progreso de instalación de características en la página Resultados indica que el rol se está instalando.  

10. En la página **Resultados** , comprueba que la instalación se realizó correctamente y, después, haz clic en el icono de advertencia con un signo de exclamación en la esquina superior derecha de la pantalla, junto a **Administrar**. En la lista Tareas, haz clic en el vínculo **Promover este servidor a controlador de dominio**.  

    > [!IMPORTANT]  
    > Si cierras el asistente de instalación en este punto en lugar de hacer clic en **Promover este servidor a controlador de dominio**, puedes continuar la instalación haciendo clic en **Tareas** en el Administrador del servidor.  

11. En la página **Configuración de implementación**, haz clic en **Agregar un nuevo bosque**, escribe el nombre del dominio raíz, **adatum.com** y haz clic en **Siguiente**.  

12. En el **opciones del controlador de dominio** página, seleccione los niveles funcionales de dominio y bosque como Windows Server 2012, especifique la contraseña de DSRM <strong>pass@word1</strong>y, a continuación, haga clic en **siguiente**.  

13. En la página **Opciones de DNS**, haz clic en **Siguiente**.  

14. En la página **Opciones adicionales** , haz clic en **Siguiente**.  

15. En la página **Rutas de acceso** , escribe las ubicaciones para la carpeta SYSVOL, archivos de registro o la base de datos de Active Directory (o acepta las ubicaciones predeterminadas) y, después, haz clic en **Siguiente**.  

16. En la página **Revisar opciones** , confirma las selecciones y, después, haz clic en **Siguiente**.  

17. En la página **Comprobación de requisitos previos**, confirma que se haya completado la validación de los requisitos previos y, después, haz clic en **Instalar**.  

18. En la página **Resultados**, comprueba que el servidor se haya configurado correctamente como controlador de dominio y haz clic en **Cerrar**.  

19. Reinicia el servidor para completar la instalación de AD DS. (De forma predeterminada, esto se produce automáticamente).  

> [!IMPORTANT]  
> Para asegurarte de que la red está correctamente configurada, después de configurar ambos bosques, haz lo siguiente:  
>   
> -   Inicia sesión en adatum.com como adatum\administrador. Abre una ventana de símbolo del sistema, escribe **nslookup contoso.com** y presiona ENTRAR.  
> -   Inicia sesión en contoso.com como contoso/administrador. Abre una ventana de símbolo del sistema, escribe **nslookup adatum.com**y presiona ENTRAR.  
>   
> Si estos comandos se ejecutan sin errores, los bosques pueden comunicarse entre sí. Para obtener más información sobre los errores de nslookup, consulte la sección sobre solución de problemas en el tema [Uso de NSlookup.exe](https://support.microsoft.com/kb/200525)  

### <a name="BKMK_2.22"></a>Establece contoso.com como bosque de confianza para adatum.com  
En este paso, creas una relación de confianza entre el sitio de Adatum Corporation y el sitio de Contoso, Ltd.  

##### <a name="to-set-contoso-as-a-trusting-forest-to-adatum"></a>Para establecer contoso.com como bosque de confianza para Adatum  

1.  Inicia sesión en DC2 como administrador. En la pantalla **Inicio** , escribe domain.msc.  

2.  En el árbol de la consola, haz clic con el botón derecho en adatum.com y, después, haz clic en Propiedades.  

3.  En la pestaña **Confianzas**, haz clic en **Nueva confianza** y, después, en **Siguiente**.  

4.  En la página **Nombre de confianza** , escribe **contoso.com**, en el campo de nombre del Sistema de nombres de dominio (DNS) y, después, haz clic en **Siguiente**.  

5.  En la página **Tipo de confianza**, haz clic en **Confianza de bosque** y, después, en **Siguiente**.  

6.  En la página **Dirección de confianza** , haz clic en **Bidireccional**.  

7.  En la página **Partes de la relación de confianza** , haz clic en **Ambos, este dominio y el dominio especificado**y, después, haz clic en **Siguiente**.  

8.  Siga las instrucciones del asistente.  

### <a name="BKMK_2.4"></a>Crear usuarios adicionales en el bosque Adatum  
Cree el usuario Jeff Low con la contraseña <strong>pass@word1</strong>y asígnele el atributo company con el valor **Adatum**.  

##### <a name="to-create-a-user-with-the-company-attribute"></a>Para crear un usuario con el atributo Company  

1.  Abre un símbolo del sistema con permisos elevados en Windows PowerShell y pega el siguiente código:  

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

### <a name="BKMK_2.5"></a>Crear el tipo de notificación Company en adataum.com  

##### <a name="to-create-a-claim-type-by-using-windows-powershell"></a>Para crear un tipo de notificación usando Windows PowerShell  

1.  Inicia sesión en adatum.com como administrador.  

2.  Abre un símbolo del sistema con permisos elevados en Windows PowerShell y escribe el siguiente código:  

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

### <a name="BKMK_2.55"></a>Habilitar la propiedad Company en contoso.com  

##### <a name="to-enable-the-company-resource-property-on-contosocom"></a>Para habilitar la propiedad Company en contoso.com  

1.  Inicia sesión en contoso.com como administrador.  

2.  En Administrador del servidor, haz clic en **Herramientas** y, después, en **Centro de administración de Active Directory**.  

3.  En el panel izquierdo del Centro de administración de Active Directory, haz clic en **Vista de árbol**. En el panel izquierdo, haz clic en **Control de acceso dinámico** y, después, haz doble clic en **Propiedades de recursos**.  

4.  Selecciona **Company** en la lista **Propiedades de recursos** y haz clic con el botón derecho en **Propiedades**. En la sección **Valores sugeridos**, haz clic en **Agregar** para agregar los valores sugeridos: Contoso y Adatum y, después, haz clic en **Aceptar** dos veces.  

5.  Selecciona **Company** en la lista **Propiedades de recursos** y haz clic con el botón derecho en **Habilitar**.  

### <a name="BKMK_2.6"></a>Habilitar el Control de acceso dinámico en adatum.com  

##### <a name="to-enable-dynamic-access-control-for-adatumcom"></a>Para habilitar el control de acceso dinámico para adatum.com  

1.  Inicia sesión en adatum.com como administrador.  

2.  Abre la Consola de administración de directivas de grupo, haz clic en **adatum.com** y, después, haz doble clic en **Controladores de dominio**.  

3.  Haz clic con el botón derecho en **Directiva predeterminada de controladores de dominio**y selecciona **Editar**.  

4.  En la ventana del Editor de administración de directivas de grupo, haz doble clic en **Configuración del equipo**, haz doble clic en **Directivas**, haz doble clic en **Plantillas administrativas**, haz doble clic en **Sistema**y, después, haz doble clic en **KDC**.  

5.  Haz doble clic en **Compatibilidad del KDC con notificaciones, autenticación compuesta y protección de Kerberos** y selecciona la opción junto a **Habilitado**. Necesitas habilitar esta opción para usar las directivas de acceso central.  

6.  Abre un símbolo del sistema con permisos elevados y ejecuta el siguiente comando:  

    ```  
    gpupdate /force  
    ```  

### <a name="BKMK_2.8"></a>Crear el tipo de notificación Company en contoso.com  

##### <a name="to-create-a-claim-type-by-using-windows-powershell"></a>Para crear un tipo de notificación usando Windows PowerShell  

1.  Inicia sesión en contoso.com como administrador.  

2.  Abre un símbolo del sistema con permisos elevados en Windows PowerShell y escribe el siguiente código:  

    ```  
    New-ADClaimType '"SourceTransformPolicy `  
    '"DisplayName 'Company' `  
    '"ID 'ad://ext/Company:ContosoAdatum' `  
    '"IsSingleValued $true `  
    '"ValueType 'string' `  

    ```  

### <a name="BKMK_2.9"></a>Crear la regla de acceso central  

##### <a name="to-create-a-central-access-rule"></a>Para crear una regla de acceso central  

1. En el panel izquierdo del Centro de administración de Active Directory, haz clic en **Vista de árbol**. En el panel izquierdo, haz clic en **Control de acceso dinámico** y, después, haz clic en **Reglas de acceso central**.  

2. Haz clic con el botón derecho en **Reglas de acceso central**, haz clic en **Nueva**y, después, en **Regla de acceso central**.  

3. En el campo **Nombre**, escribe **AdatumEmployeeAccessRule**.  

4. En la sección **Permisos**, selecciona la opción **Usar los siguientes permisos como permisos actuales**, haz clic en **Editar** y, después, haz clic en **Agregar**. Haz clic en el vínculo **Seleccionar una entidad de seguridad** , escribe **Usuarios autenticados**y, después, haz clic en **Aceptar**.  

5. En el cuadro de diálogo **Entrada de permiso para permisos**, haga clic en **Agregar una condición** y especifique las condiciones siguientes:  [**Usuario**] [**Empresa**] [**Igual**] [**Valor**] [**Adatum**]. Los permisos deben ser **Modificar, Leer y ejecutar, Leer, Escribir**.  

6. Haga clic en **Aceptar**.  

7. Haz clic en **Aceptar** tres veces para terminar y volver al Centro de administración de Active Directory.  

   ![guías de solución](media/Appendix-B--Setting-Up-the-Test-Environment/PowerShellLogoSmall.gif)***<em>comandos equivalentes de Windows PowerShell</em>***  

   Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  

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

1.  Inicia sesión en contoso.com como administrador.  

2.  Abre un símbolo del sistema con permisos elevados en Windows PowerShell y pega el siguiente código:  

    ```  
    New-ADCentralAccessPolicy "Adatum Only Access Policy"   
    Add-ADCentralAccessPolicyMember "Adatum Only Access Policy" `  
    -Member "AdatumEmployeeAccessRule" `  
    ```  

### <a name="BKMK_2.11"></a>Publicar la nueva directiva mediante Directiva de grupo  

##### <a name="to-apply-the-central-access-policy-across-file-servers-through-group-policy"></a>Para aplicar la directiva de acceso central a todos los servidores de archivos mediante directiva de grupo  

1.  En la pantalla **Inicio** , escriba **Herramientas administrativas**y, en la barra **Buscar** , haga clic en **Configuración**. En los resultados de **Configuración** , haga clic en **Herramientas administrativas**. Abre la Consola de administración de directivas de grupo de la carpeta **Herramientas administrativas** .  

    > [!TIP]  
    > Si la opción **Mostrar herramientas administrativas** está deshabilitada, no aparecerá la carpeta Herramientas administrativas ni su contenido en los resultados de **Configuración**.  

2.  Haga clic en el dominio contoso.com, haga clic en **crear un GPO en este dominio y vincularlo aquí**  

3.  Escribe un nombre descriptivo para el GPO, como **AdatumAccessGPO**, y haz clic en **Aceptar**.  

##### <a name="to-apply-the-central-access-policy-to-the-file-server-through-group-policy"></a>Para aplicar la directiva de acceso central al servidor de archivos mediante directiva de grupo  

1.  En la pantalla **Inicio**, escribe **Administración de directivas de grupo** en el cuadro **Buscar**. Abre **Administración de directivas de grupo** desde la carpeta Herramientas administrativas.  

    > [!TIP]  
    > Si la opción **Mostrar herramientas administrativas** está deshabilitada, no aparecerá la carpeta Herramientas administrativas ni su contenido en los resultados de Configuración.  

2.  Ve a Contoso y selecciónalo de la siguiente manera: Administración de directivas de grupo\Bosque: contoso.com\Dominios\contoso.com.  

3.  Haz clic con el botón derecho en la directiva **AdatumAccessGPO** y selecciona **Editar**.  

4.  En el Editor de administración de directivas de grupo, haz clic en **Configuración del equipo**, expande **Directivas**, expande **Configuración de Windows**y, después, haz clic en **Configuración de seguridad**.  

5.  Expande **Sistema de archivos**, haz clic con el botón derecho en **Directiva de acceso central** y, después, haz clic en **Administrar directivas de acceso central**.  

6.  En el cuadro de diálogo **Configuración de directivas de acceso central** , haz clic en **Agregar**, selecciona **Adatum Only Access Policy**y, después, haz clic en **Aceptar**.  

7.  Cierra el Editor de administración de directivas de grupo. Ya has agregado la directiva de acceso central a la directiva de grupo.  

### <a name="BKMK_2.12"></a>Cree la carpeta Earnings en el servidor de archivos  
Crea un nuevo volumen NTFS en FILE1 y crea la siguiente carpeta: D:\Earnings.  

> [!NOTE]  
> Las directivas de acceso central no están habilitadas de forma predeterminada en el volumen C: de arranque o del sistema.  

### <a name="BKMK_2.13"></a>Establecer la clasificación y aplicar la directiva de acceso central en la carpeta Earnings  

##### <a name="to-assign-the-central-access-policy-on-the-file-server"></a>Para asignar la directiva de acceso central en el servidor de archivos  

1. En el Administrador de Hyper-V, conecta con el servidor FILE1. Inicie sesión el servidor usando Contoso\Administrador con la contraseña <strong>pass@word1</strong>.  

2. Abra un símbolo del sistema con privilegios elevados y escriba: **gpupdate /force**. Así te aseguras de que los cambios en la directiva de grupo surtan efecto en el servidor.  

3. También tienes que actualizar las propiedades de recursos globales desde Active Directory. Abra Windows PowerShell, escriba `Update-FSRMClassificationpropertyDefinition`y presione ENTRAR. Cierra Windows PowerShell.  

4. Abre el Explorador de Windows y ve a D:\EARNINGS. Haz clic con el botón derecho en la carpeta **Earnings** y haz clic en **Propiedades**.  

5. Haz clic en la pestaña **Clasificación**. Selecciona Companyy, después, selecciona **Adatum** en el campo **Valor** .  

6. Haz clic en **Cambiar**, selecciona **Adatum Only Access Policy** en el menú desplegable y, después, haz clic en **Aplicar**.  

7. Haz clic en la pestaña **Seguridad**, en **Opciones avanzadas** y, después, haz clic en la pestaña **Directiva central**. Deberías ver **AdatumEmployeeAccessRule** en la lista. Puedes expandir el elemento para ver todos los permisos que estableciste cuando creaste la regla en Active Directory.  

8. Haz clic en **Aceptar** para volver al Explorador de Windows.  



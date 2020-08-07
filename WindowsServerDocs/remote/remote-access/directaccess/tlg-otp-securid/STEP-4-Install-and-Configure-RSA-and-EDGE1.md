---
title: Paso 4 Instalación y configuración de RSA y EDGE1
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de DirectAccess con autenticación OTP y RSA SecurID para Windows Server 2016'
manager: brianlic
ms.topic: article
ms.assetid: d46ede6f-1a21-414d-b8c3-6b5c87344b9d
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1c970a3782286c18f64813c6d1e92ad363870a59
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87963991"
---
# <a name="step-4-install-and-configure-rsa-and-edge1"></a>Paso 4 Instalación y configuración de RSA y EDGE1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

RSA es el servidor RADIUS y OTP, y se instala antes de configurar RADIUS y OTP.

Realizará los siguientes pasos para configurar la implementación de RSA:

1. Instale el sistema operativo en el servidor de RSA. Instale Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 en el servidor de RSA.

2. Configurar TCP/IP en RSA. Configure los valores de TCP/IP en el servidor de RSA.

3. Copie los archivos de instalación del administrador de autenticación en el servidor de RSA. Después de instalar el sistema operativo en RSA, copie los archivos del administrador de autenticación en el equipo de RSA.

4. Una el servidor RSA al dominio CORP. Únase a RSA en el dominio CORP.

5. Deshabilite el Firewall de Windows en RSA. Deshabilite el Firewall de Windows en el servidor de RSA.

6. Instale el administrador de autenticación de RSA en el servidor de RSA. Instale el administrador de autenticación de RSA.

7. Configure el administrador de autenticación de RSA. Configure el administrador de autenticación.

8. Cree DAProbeUser. Cree una cuenta de usuario con fines de sondeo.

9. Instale el token de software RSA SecurID en cliente1. Instale el token de software RSA SecurID en cliente1.

10. Configure EDGE1 como un agente de autenticación RSA. Configure el agente de autenticación RSA en EDGE1.

11. Configure EDGE1 para admitir la autenticación de OTP. Configure OTP para DirectAccess y Compruebe la configuración.

## <a name="install-the-operating-system-on-the-rsa-server"></a><a name="InstallOS"></a>Instalación del sistema operativo en el servidor de RSA

1.  En RSA, inicie la instalación de Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.

2.  Siga las instrucciones para completar la instalación, especificando Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (instalación completa) y una contraseña segura para la cuenta de administrador local. Inicie sesión con la cuenta Administrador local.

3.  Conecte RSA a una red que tenga acceso a Internet y ejecute Windows Update para instalar las actualizaciones más recientes de Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 y, a continuación, desconéctese de Internet.

4.  Conecte RSA a la subred de la red corporativa.

## <a name="configure-tcpip-on-rsa"></a><a name="TCP"></a>Configuración de TCP/IP en RSA

1.  En Tareas de configuración inicial, haga clic en **configurar redes**.

2.  En **conexiones de red**, haga clic con el botón secundario en **conexión de área local**y, a continuación, haga clic en **propiedades**.

3.  Haga clic en **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, en **Propiedades**.

4.  Haga clic en **Usar la siguiente dirección IP**. En **Dirección IP**, escribe **10.0.0.5**. En **Máscara de subred**, escriba **255.255.255.0**. En **puerta de enlace predeterminada**, escriba **10.0.0.2**. Haga clic en **usar las siguientes direcciones de servidor DNS**, en **servidor DNS preferido**, escriba **10.0.0.1**.

5.  Haga clic en ** Opciones avanzadas** y, a continuación, haga clic en la pestaña **DNS**.

6.  En **sufijo DNS para esta conexión**, escriba **Corp.contoso.com**y, a continuación, haga clic en **Aceptar** dos veces.

7.  En el cuadro de diálogo **propiedades de conexión de área local** , haga clic en **cerrar**.

8.  Cierre la ventana **Conexiones de red**.

## <a name="copy-authentication-manager-installation-files-to-the-rsa-server"></a><a name="copyinstfiles"></a>Copiar los archivos de instalación del administrador de autenticación en el servidor de RSA

1.  En el servidor de RSA, cree la carpeta C:\RSA instalación de.

2.  Copie el contenido de los medios RSA Authentication Manager 7,1 SP4 en la carpeta de instalación C:\RSA

3.  Cree la subcarpeta C:\RSA Installation\License y token.

4.  Copie los archivos de licencia RSA en C:\RSA Installation\License y token.

## <a name="join-the-rsa-server-to-the-corp-domain"></a><a name="JoinDomain"></a>Unir el servidor de RSA al dominio CORP

1.  Haga clic con el botón secundario en **Mi PC** y haga clic en **Propiedades**.

2.  En el cuadro de diálogo **Propiedades del sistema**, en la pestaña **Nombre de equipo**, haz clic en **Cambiar**.

3.  En **nombre de equipo**, escriba **RSA**. En **miembro de**, haga clic en **dominio**, escriba **Corp.contoso.com**y haga clic en **Aceptar**.

4.  Cuando se le pida un nombre de usuario y una contraseña, escriba **user1** y su contraseña y haga clic en **Aceptar**.

5.  En el cuadro de diálogo de bienvenida del dominio, haga clic en **Aceptar**.

6.  Cuando se le indique que debe reiniciar el equipo, haga clic en **Aceptar**.

7.  En el cuadro de diálogo **Propiedades del sistema**, haga clic en **Cerrar**.

8.  Cuando se le pida que reinicie el equipo, haga clic en **Reiniciar ahora**.

9. Una vez reiniciado el equipo, escriba **user1** y la contraseña, seleccione Corp en la lista desplegable **iniciar sesión en:** y haga clic en **Aceptar**.

## <a name="disable-windows-firewall-on-rsa"></a><a name="BKMK_Firewall"></a>Deshabilitar Firewall de Windows en RSA

1.  Haga clic en **Inicio**, en **Panel de control**, en **sistema y seguridad**y, a continuación, en **firewall de Windows**.

2.  Haga clic en **activar o desactivar Firewall de Windows**.

3.  **Desactivar Firewall de Windows** para todas las configuraciones.

4.  Haga clic en **Aceptar** y cierre Firewall de Windows.

## <a name="install-rsa-authentication-manager-on-the-rsa-server"></a><a name="install"></a>Instalación del administrador de autenticación de RSA en el servidor de RSA

1.  Si aparece el mensaje de advertencia de seguridad en cualquier momento durante este proceso, haga clic en **Ejecutar** para continuar.

2.  Abra la carpeta de instalación C:\RSA y haga doble clic en **autorun.exe**.

3.  Haga clic en **instalar ahora**, haga clic en **siguiente**, seleccione la opción superior para América y haga clic en **siguiente**.

4.  Seleccione Acepto **los términos del contrato de licencia**y haga clic en **siguiente**.

5.  Seleccione **instancia principal**y haga clic en **siguiente**.

6.  En el campo **nombre de directorio:** , escriba **C:\RSA**y haga clic en **siguiente**.

7.  Compruebe que el nombre del servidor (RSA.corp.contoso.com) y la dirección IP sean correctos y haga clic en **siguiente**.

8.  Vaya a C:\RSA Installation\License y token y haga clic en **siguiente**.

9. En la página **comprobar el archivo de licencia** , haga clic en **siguiente**.

10. En el campo **ID. de usuario** , escriba **Administrador**y en los campos **contraseña** y **Confirmar contraseña** escriba una contraseña segura. Haga clic en **Next**.

11. En la pantalla de selección de registro, acepte los valores predeterminados y haga clic en **siguiente**.

12. En la pantalla resumen, haga clic en **instalar**.

13. Una vez finalizada la instalación, haga clic en **Finalizar**.

## <a name="configure-rsa-authentication-manager"></a><a name="confiauthmgr"></a>Configurar el administrador de autenticación de RSA

1.  Si la consola de seguridad de RSA no se abre automáticamente, en el escritorio de equipo de RSA, haga doble clic en "consola de seguridad RSA".

2.  Si aparece la alerta de advertencia/seguridad del certificado de seguridad, haga clic en **pasar a este sitio web** o haga clic en **sí** para continuar y agregar este sitio a sitios de confianza, si se solicita.

3.  En el campo ID. de **usuario** , escriba **Administrador** y haga clic en **Aceptar**.

4.  En el campo **contraseña** , escriba la contraseña de la cuenta de administrador y haga clic en **iniciar sesión**.

5.  Insertar información de token.

    1.  En la **Consola RSA Security** , haga clic en **autenticación** y en **tokens de SecurID**.

    2.  Haga clic en el **trabajo tokens de importación**y, a continuación, en **Agregar nuevo**.

    3.  En la sección **Opciones de importación** , haga clic en **examinar**. Busque y seleccione el archivo XML de tokens en C:\ Installation\License de RSA y la carpeta de tokens y haga clic en **abrir**.

    4.  Haga clic en **Enviar trabajo** en la parte inferior de la página.

6.  Cree un nuevo usuario de OTP.

    1.  En la **Consola RSA Security** , haga clic en la pestaña **identidad** , haga clic en **usuarios**y, a continuación, en **Agregar nuevo**.

    2.  En la sección **Last Name:** escriba **User**y, en la sección **User ID:** , escriba **user1** (userid debe ser el mismo que el nombre de usuario de ad usado para este laboratorio).  En las secciones **contraseña:** y **Confirmar contraseña:** escriba una contraseña segura. Desactive la casilla **"requerir al usuario que cambie la contraseña en el siguiente inicio de sesión"** y haga clic en **Guardar**.

7.  Asigne user1 a uno de los tokens importados.

    1.  En la página **usuarios** , haga clic en **user1** y en **tokens de SecurID**.

    2.  Haga clic en **tokens de SecurID** y en **asignar token**.

    3.  En el encabezado **número de serie** , haga clic en el primer número que aparece y haga clic en **asignar**.

    4.  Haga clic en el token asignado y haga clic en **Editar**. En la sección **Administración de PIN de SecurID** para el requisito de autenticación de **usuario**, seleccione **no requerir PIN (solo código de token)**.

    5.  Haga clic en **Guardar y distribuir token**.

    6.  En la página **distribuir el token de software** de la sección **aspectos básicos** , haga clic en **emitir archivo de token (SDTID)**.

    7.  En la página **distribuir el token de software** de la sección Opciones de archivo de **token** , desactive la casilla **Habilitar protección contra copia** . Haga clic en **sin contraseña** y en **siguiente**.

    8.  En la página **distribuir el token de software** de la sección **Descargar archivo** , haga clic en **Descargar ahora**. Haga clic en **Guardar**. Vaya a la instalación de C:\RSA y haga clic en **Guardar** y **cerrar**.

    9. Minimice la **consola de seguridad de RSA** para usarla más adelante.

8.  Configure el administrador de autenticación como servidor RADIUS.

    1.  En el escritorio del equipo de RSA, haga doble clic en **"consola del operador de seguridad RSA"**.

    2.  Si aparece la alerta de advertencia/seguridad del certificado de seguridad, haga clic en **pasar a este sitio web** o haga clic en **sí** para continuar y agregar este sitio a sitios de confianza si se solicita.

    3.  Escriba el ID. de usuario y la contraseña y haga clic en **iniciar sesión**.

    4.  Haga clic en **Deployment Configuration-RADIUS-configure Server**.

    5.  En la página **credenciales adicionales requerida** , escriba el identificador de usuario y la contraseña de administrador y haga clic en **Aceptar**.

    6.  En la página **configurar servidor RADIUS** , escriba la misma contraseña usada para el usuario administrador de los **secretos** y la **contraseña maestra**. Escriba el identificador de usuario y la contraseña de administrador y haga clic en **configurar**.

    7.  Compruebe que se muestra el mensaje "se ha **configurado correctamente el servidor RADIUS"** . Haga clic en **Done**(Listo). Cierre la **consola del operador de RSA**.

    8.  Vuelva a la **Consola RSA Security Console**.

    9. En la pestaña **RADIUS** , haga clic en **servidores RADIUS**. Compruebe que rsa.corp.contoso.com aparece en la lista.

9. Configure el servidor de RSA como cliente de autenticación RSA.

    1.  En la pestaña **RADIUS** , haga clic en **clientes RADIUS** y **Agregar nuevo**.

    2.  Haga clic en la casilla **cualquier cliente RADIUS** .

    3.  Escriba una contraseña segura de su elección en el campo **secreto compartido** . Usará esta misma contraseña más adelante al configurar EDGE1 para OTP.

    4.  Deje en blanco el campo **dirección IP** y la entrada **marca/modelo** como **RADIUS estándar**.

    5.  Haga clic en **guardar sin el agente RSA**.

10. Cree los archivos necesarios para configurar EDGE1 como un agente de autenticación RSA.

    1.  En la pestaña **acceso** , resalte **agentes de autenticación**y haga clic en **Agregar nuevo**.

    2.  Escriba **EDGE1** en el campo **nombre de host** y haga clic en **resolver IP**.

    3.  Observe que la dirección IP de EDGE1 se muestra ahora en el campo **dirección IP** . Haga clic en **Guardar**.

11. Genere un archivo de configuración para el servidor de EDGE1 (AM_Config.zip).

    1.  En la pestaña **acceso** , resalte **agentes de autenticación**y haga clic en **generar archivo de configuración**.

    2.  En la página **generar archivo de configuración** , haga clic en **generar archivo**de configuración y, a continuación, haga clic en **Descargar ahora**.

    3.  Haga clic en **Guardar**, desplácese hasta C:\ Instalación de RSA y haga clic en **Guardar**.

    4.  Haga clic en **cerrar** en el cuadro de diálogo **Descarga completada** .

12. Genere un archivo de secreto de nodo para el servidor de EDGE1 (EDGE1_NodeSecret.zip).

    1.  En la pestaña **acceso** , resalte **agentes de autenticación**y haga clic en **administrar existente**.

    2.  Haga clic en el nodo configurado EDGE1 actual y haga clic en **administrar secreto de nodo**.

    3.  Active la casilla **crear un nuevo secreto de nodo aleatorio y exportar el secreto del nodo a un archivo** .

    4.  Escriba la misma contraseña usada para el usuario administrador en los campos **contraseña de cifrado** y **Confirmar contraseña de cifrado** y haga clic en **Guardar**.

    5.  En la página se **generó el archivo de secreto de nodo** , haga clic en **Descargar ahora**.

    6.  En el cuadro de diálogo **descarga de archivos** , haga clic en **Guardar**, desplácese a la instalación de C:\RSA y haga clic en **Guardar**. Haga clic en **cerrar** en el cuadro de diálogo **Descarga completada** .

    7.  Desde la copia multimedia de RSA Authentication Manager \auth_mgr\windows-x86_64\am\rsa-ace_nsload\win32-5.0-x86\agent_nsload.exe a la instalación de C:\RSA.

## <a name="create-daprobeuser"></a><a name="BKMK_DAProbeUser"></a>Crear DAProbeUser

1.  En la **Consola RSA Security** , haga clic en la pestaña **identidad** , haga clic en **usuarios**y, a continuación, en **Agregar nuevo**.

2.  En la sección **Last Name:** , escriba **Probe**y, en la sección **User ID:** , escriba **DAProbeUser**. En las secciones **contraseña:** y **Confirmar contraseña:** escriba una contraseña segura. Desactive la casilla **"requerir al usuario que cambie la contraseña en el siguiente inicio de sesión"** y haga clic en **Guardar**.

## <a name="install-rsa-securid-software-token-on-client1"></a><a name="InstToken"></a>Instalación del token de software RSA SecurID en CLIENT1
Utilice este procedimiento para instalar el token de software de SecurID en cliente1.

#### <a name="install-securid-software-token"></a>Instalación del token de software de SecurID

1.  En el equipo CLIENT1, cree la carpeta C:\RSA files. Copie el archivo Software_Tokens.zip desde la instalación de C:\RSA en el equipo RSA a C:\RSA files. Extraiga el archivo User1_000031701832. SDTID en C:\RSA files en cliente1.

2.  Acceda al origen multimedia del token de software de RSA SecurID y haga doble clic en RSASECURIDTOKEN410 en la carpeta de la **aplicación cliente de SoftwareToken de SecurID** para iniciar la instalación de RSA SecurID. Si aparece el mensaje **de advertencia de seguridad abrir archivo** , haga clic en **Ejecutar**.

3.  En el cuadro de diálogo **Asistente InstallShield de token de software de RSA SecurID,** haga clic en **siguiente** dos veces.

4.  Acepte el contrato de licencia y haga clic en **siguiente**.

5.  En el cuadro de diálogo **tipo de instalación** , seleccione **típico**, haga clic en **siguiente**y haga clic en **instalar**.

6.  Si aparece el cuadro de diálogo **Control de cuentas de usuario**, confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.

7.  Active la casilla **Launch RSA SecurID software token** y haga clic en **Finish (finalizar**).

8.  Haga clic en **Importar desde archivo**.

9. Haga clic en **examinar**, seleccione C:\RSA files \ USER1_000031701832. SDTID y haga clic en **Open (abrir**).

10. Haga clic en **Aceptar** dos veces.

## <a name="configure-edge1-as-an-rsa-authentication-agent"></a><a name="configAuthAgt"></a>Configuración de EDGE1 como agente de autenticación RSA
Use este procedimiento para configurar EDGE1 para realizar la autenticación RSA.

#### <a name="configure-the-rsa-authentication-agent"></a>Configuración del agente de autenticación RSA

1. En EDGE1, abra el explorador de Windows y cree la carpeta C:\RSA files. Busque el disco de instalación de RSA ACE.

2. Copie los archivos agent_nsload.exe, AM_Config.zip y EDGE1_NodeSecret.zip desde el medio de RSA a los archivos C:\RSA.

3. Extraiga el contenido de ambos archivos zip en las siguientes ubicaciones:

   1.  C:\windows\system32

   2.  C:\Windows\SysWOW64

4. Copie agent_nsload.exe en C:\Windows\SysWOW64 \\ .

5. Abra un símbolo del sistema con privilegios elevados y vaya a C:\Windows\SysWOW64.

6. Escriba **agent_nsload.exe-f nodesecret. REC-p <password> ** donde <password> es la contraseña segura que creó durante la configuración inicial de RSA. Presiona Entrar.

7. Copie C:\Windows\SysWOW64\securid en C:\Windows\System32.

## <a name="configure-edge1-to-support-otp-authentication"></a><a name="configOTP"></a>Configuración de EDGE1 para admitir la autenticación OTP
Use este procedimiento para configurar OTP para DirectAccess y comprobar la configuración.

#### <a name="configure-otp-for-directaccess"></a>Configuración de OTP para DirectAccess

1.  En EDGE1, abra Administrador del servidor y haga clic en **acceso remoto** en el panel izquierdo.

2.  Haga clic con el botón secundario en **EDGE1** en el panel servidores y seleccione **Administración de acceso remoto**.

3.  Haga clic en **Configuración**.

4.  En la ventana **configuración de DirectAccess** , en **paso 2: servidor de acceso remoto**, haga clic en **Editar**.

5.  Haga clic en **siguiente** tres veces y, en la sección **autenticación** , seleccione **dos factores autenticación** y **usar OTP**y asegúrese de que la opción **usar certificados de equipo** está activada. Compruebe que la entidad de certificación raíz está establecida en **CN = Corp-app1-CA**. Haga clic en **Next**.

6.  En la sección **servidor RADIUS OTP** , haga doble clic en el campo **nombre del servidor** en blanco.

7.  En el cuadro de diálogo **Agregar un servidor RADIUS** , escriba **RSA** en el campo **nombre del servidor** . Haga clic en **cambiar** junto al campo **secreto compartido** y escriba la misma contraseña que usó al configurar los clientes RADIUS en el servidor de RSA en los campos **nuevo secreto** y **confirmar nuevo secreto** . Haga clic en **Aceptar** dos veces y haga clic en **siguiente**.

    > [!NOTE]
    > Si el servidor RADIUS está en un dominio diferente al del servidor de acceso remoto, el campo **nombre del servidor** debe especificar el FQDN del servidor RADIUS.

8.  En la sección **servidores de CA de OTP** , seleccione app1.Corp.contoso.com y haga clic en **Agregar**. Haga clic en **Next**.

9. En la **Página plantillas de certificado de OTP** , haga clic en **examinar** para seleccionar una plantilla de certificado que se usa para la inscripción de certificados que se emiten para la autenticación de OTP y, en el cuadro de diálogo **plantillas de certificado** , seleccione **DAOTPLogon**. Haga clic en **Aceptar**. Haga clic en **examinar** para seleccionar una plantilla de certificado que se usa para inscribir el certificado usado por el servidor de acceso remoto para firmar las solicitudes de inscripción de certificados OTP y, en el cuadro de diálogo **plantillas de certificado** , seleccione **DAOTPRA**. Haga clic en **Aceptar**. Haga clic en **Next**.

10. En la página **instalación del servidor de acceso remoto** , haga clic en **Finalizar**y en **Finalizar** en el **Asistente para el experto de DirectAccess**.

11. En el cuadro de diálogo **revisión de acceso remoto** , haga clic en **aplicar**, espere a que se actualice la Directiva de DirectAccess y haga clic en **cerrar**.

12. En la pantalla **Inicio** , escriba**powershell.exe**, haga clic con el botón derecho en **PowerShell**, haga clic en **Opciones avanzadas**y haga clic en **Ejecutar como administrador**. Si aparece el cuadro de diálogo **Control de cuentas de usuario**, confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.

13. En la ventana de Windows PowerShell, escriba **gpupdate/force** y presione Entrar.

14. Cierre y vuelva a abrir la consola de administración de acceso remoto y compruebe que la configuración de OTP sea correcta.




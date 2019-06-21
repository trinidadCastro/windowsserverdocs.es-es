---
title: PASO 4 instalar y configurar RSA y EDGE1
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar DirectAccess con autenticación OTP y RSA SecurID para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d46ede6f-1a21-414d-b8c3-6b5c87344b9d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5280a02568305512f868f559fe35d11dc618f2b4
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283072"
---
# <a name="step-4-install-and-configure-rsa-and-edge1"></a>PASO 4 instalar y configurar RSA y EDGE1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

RSA es el servidor RADIUS y OTP y se instala antes de configurar la OTP y RADIUS.  
  
Llevará a cabo los pasos siguientes para configurar la implementación de RSA:  
  
1. Instalar el sistema operativo en el servidor RSA. Instale Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 en el servidor RSA.  
  
2. Configurar TCP/IP en RSA. Configuración de TCP/IP en el servidor RSA.  
  
3. Copie los archivos de instalación de administrador de autenticación en el servidor RSA. Después de instalar el sistema operativo en RSA, copie los archivos del Administrador de autenticación en el equipo RSA.  
  
4. Unir el servidor RSA para el dominio CORP. Únase a RSA al dominio CORP.  
  
5. Deshabilitar Firewall de Windows en RSA. Deshabilite el Firewall de Windows en el servidor RSA.  
  
6. RSA Authentication Manager se instale en el servidor RSA. Instalar a Administrador de autenticación de RSA.  
  
7. Configurar el Administrador de autenticación de RSA. Configurar el Administrador de autenticación.  
  
8. Crear DAProbeUser. Cree una cuenta de usuario para el sondeo de propósitos.  
  
9. Instale el token de software de RSA SecurID en CLIENT1. Instale el token de software de RSA SecurID en CLIENT1.  
  
10. Configurar EDGE1 como un agente de autenticación de RSA. Configurar el agente de autenticación de RSA en EDGE1.  
  
11. Configurar EDGE1 para admitir la autenticación de OTP. Configurar la OTP de DirectAccess y compruebe la configuración.  
  
## <a name="InstallOS"></a>Instalar el sistema operativo en el servidor RSA  
  
1.  En RSA, inicie la instalación de Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
2.  Siga las instrucciones para completar la instalación, especificando Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (instalación completa) y una contraseña segura para la cuenta de administrador local. Inicie sesión con la cuenta Administrador local.  
  
3.  Conecte RSA a una red que tenga acceso a Internet y ejecute Windows Update para instalar las actualizaciones más recientes para Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 y, a continuación, desconectarse de Internet.  
  
4.  RSA se conecte a la subred Corpnet.  
  
## <a name="TCP"></a>Configurar TCP/IP en RSA  
  
1.  Tareas de configuración inicial, haga clic en **configurar redes**.  
  
2.  En **las conexiones de red**, haga clic en **Local Area Connection**y, a continuación, haga clic en **propiedades**.  
  
3.  Haga clic en **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, en **Propiedades**.  
  
4.  Haga clic en **Usar la siguiente dirección IP**. En **Dirección IP**, escriba **10.0.0.5**. En **Máscara de subred**, escriba **255.255.255.0**. En **puerta de enlace predeterminada**, tipo **10.0.0.2**. Haga clic en **usar las siguientes direcciones de servidor DNS**, en **servidor DNS preferido**, tipo **10.0.0.1**.  
  
5.  Haga clic en **Opciones avanzadas** y, a continuación, haga clic en la pestaña **DNS**.  
  
6.  En **sufijo DNS para esta conexión**, tipo **corp.contoso.com**y, a continuación, haga clic en **Aceptar** dos veces.  
  
7.  En el **propiedades de conexión de área Local** cuadro de diálogo, haga clic en **cerrar**.  
  
8.  Cierre la ventana **Conexiones de red**.  
  
## <a name="copyinstfiles"></a>Copie los archivos de instalación de administrador de autenticación en el servidor RSA  
  
1.  En el servidor RSA cree la carpeta de instalación C:\RSA.  
  
2.  Copie el contenido de los medios de RSA Authentication Manager 7.1 SP4 en la carpeta de instalación C:\RSA.  
  
3.  Crear la subcarpeta C:\RSA Installation\License y símbolo (token).  
  
4.  Copie los archivos de licencia RSA para C:\RSA Installation\License y Token.  
  
## <a name="JoinDomain"></a>Unir el servidor RSA para el dominio CORP  
  
1.  Haga clic en **Mi PC**y haga clic en **propiedades**.  
  
2.  En el cuadro de diálogo **Propiedades del sistema**, en la pestaña **Nombre del equipo**, haga clic en **Cambiar**.  
  
3.  En **nombre_equipo**, tipo **RSA**. En **miembro de**, haga clic en **dominio**, tipo **corp.contoso.com**y haga clic en **Aceptar**.  
  
4.  Cuando se le pida un nombre de usuario y contraseña, escriba **User1** y su contraseña y haga clic en **Aceptar**.  
  
5.  En el cuadro de diálogo le da la bienvenida de dominio, haga clic en **Aceptar**.  
  
6.  Cuando se le indique que debe reiniciar el equipo, haga clic en **Aceptar**.  
  
7.  En el cuadro de diálogo **Propiedades del sistema** , haga clic en **Cerrar**.  
  
8.  Cuando se le pida que reinicie el equipo, haga clic en **Reiniciar ahora**.  
  
9. Una vez reiniciado el equipo, escriba **User1** y la contraseña, seleccione CORP en el **iniciar sesión en:** lista desplegable y haga clic en **Aceptar**.  
  
## <a name="BKMK_Firewall"></a>Deshabilitar el Firewall de Windows en RSA  
  
1.  Haga clic en **iniciar**, haga clic en **Panel de Control**, haga clic en **sistema y seguridad**y haga clic en **Windows Firewall**.  
  
2.  Haga clic en **activar o desactivar Firewall de Windows**.  
  
3.  **Desactivar Firewall de Windows** para todas las configuraciones.  
  
4.  Haga clic en **Aceptar** y cierre el Firewall de Windows.  
  
## <a name="install"></a>Instale a RSA Authentication Manager en el servidor RSA  
  
1.  Si aparece el mensaje de advertencia de seguridad en cualquier momento durante este proceso, haga clic en **ejecutar** para continuar.  
  
2.  Abra la carpeta de instalación de C:\RSA y haga doble clic en **autorun.exe**.  
  
3.  Haga clic en **instalar ahora**, haga clic en **siguiente**, seleccione la opción superior para el continente americano y haga clic en **siguiente**.  
  
4.  Seleccione **acepto los términos del contrato de licencia**y haga clic en **siguiente**.  
  
5.  Seleccione **instancia principal**y haga clic en **siguiente**.  
  
6.  En el **nombre de directorio:** tipo de campo **C:\RSA**y haga clic en **siguiente**.  
  
7.  Compruebe que la dirección IP y el nombre del servidor (RSA.corp.contoso.com) son correctos y haga clic en **siguiente**.  
  
8.  Vaya a C:\RSA Installation\License y símbolo (token) y haga clic en **siguiente**.  
  
9. En el **comprobar si el archivo licencia** página, haga clic en **siguiente**.  
  
10. En el **Id. de usuario** tipo de campo **administrador**y en el **contraseña** y **Confirmar contraseña** campos escriba una contraseña segura. Haz clic en **Siguiente**.  
  
11. En la pantalla de selección de registro, acepte los valores predeterminados y haga clic en **siguiente**.  
  
12. En la pantalla Resumen, haga clic en **instalar**.  
  
13. Una vez completada la instalación, haga clic en **finalizar**.  
  
## <a name="confiauthmgr"></a>Configurar el Administrador de autenticación de RSA  
  
1.  Si la consola de seguridad de RSA no se abre automáticamente, a continuación, en el escritorio del equipo RSA haga doble clic en "Consola de RSA Security".  
  
2.  Si la seguridad de advertencia de certificado y aparece la alerta de seguridad, haga clic en **pasar a este sitio Web** o haga clic en **Sí** para continuar y agregar este sitio a sitios de confianza, si se solicita.  
  
3.  En el **Id. de usuario** tipo de campo **administrador** y haga clic en **Aceptar**.  
  
4.  En el **contraseña** campo tipo de la contraseña de la cuenta de administrador y haga clic en **iniciar sesión**.  
  
5.  Insertar información de Token.  
  
    1.  En el **consola de RSA Security** haga clic en **autenticación** y haga clic en **SecurID Tokens**.  
  
    2.  Haga clic en **trabajo de importación Tokens**y, a continuación, haga clic en **Agregar nuevo**.  
  
    3.  En el **opciones de importación** sección, haga clic **examinar**. Busque y seleccione el archivo XML de los tokens en el C:\ RSA Installation\License y carpeta símbolo (token) y haga clic en **abierto**.  
  
    4.  Haga clic en **enviar trabajo** en la parte inferior de la página.  
  
6.  Crear nuevo usuario OTP.  
  
    1.  En el **consola de RSA Security** haga clic en el **identidad** , haga clic **usuarios**y haga clic en **Agregar nuevo**.  
  
    2.  En el **Last Name:** sección tipo **usuario**y en el **Id. de usuario:** sección tipo **User1** (identificador de usuario debe ser el mismo que el nombre de usuario de AD usado para Este laboratorio).  En el **contraseña:** y **Confirmar contraseña:** secciones escriba una contraseña segura. Desactive el **'Que el usuario cambie la contraseña en el siguiente inicio de sesión'** casilla de verificación y haga clic en **guardar**.  
  
7.  Asignar Usuario1 a uno de los tokens importados.  
  
    1.  En el **usuarios** página haga clic en **User1** y haga clic en **SecurID Tokens**.  
  
    2.  Haga clic en **SecurID Tokens** y haga clic en **asignar Token**.  
  
    3.  En el **número de serie** encabezado haga clic en el primer número que aparece y haga clic en **asignar**.  
  
    4.  Haga clic en el token asignado y haga clic en **editar**. En el **SecurID PIN administración** sección **el requisito de autenticación de usuario**, seleccione **no requiere un PIN (solo código de token)** .  
  
    5.  Haga clic en **guardar y distribuir Token**.  
  
    6.  En el **distribuir el Token de Software** página en el **Fundamentos** sección, haga clic en **archivo símbolo (token) de problema (SDTID)** .  
  
    7.  En el **Token de Software distribuir** página en el **opciones de archivo de Token** sección, desactive la **Habilitar protección contra copia** casilla de verificación. Haga clic en **ninguna contraseña** y **siguiente**.  
  
    8.  En el **Token de Software distribuir** página en el **Download File** sección, haga clic en **Descargar ahora**. Haga clic en **Guardar**. Vaya a la instalación de C:\RSA y haga clic en **guardar** y **cerrar**.  
  
    9. Minimizar la **RSA Security consola** para su uso posterior.  
  
8.  Configurar el Administrador de autenticación como servidor RADIUS.  
  
    1.  En el doble clic en RSA equipo escritorio **"Consola de operaciones de seguridad de RSA"** .  
  
    2.  Si la seguridad de advertencia de certificado y aparece la alerta de seguridad, haga clic en **pasar a este sitio Web** o haga clic en **Sí** para continuar y agregar este sitio a sitios de confianza si se solicita.  
  
    3.  Escriba el Id. de usuario y la contraseña y haga clic en **iniciar sesión**.  
  
    4.  Haga clic en **configuración de implementación - RADIUS - configurar servidor**.  
  
    5.  En el **credenciales adicionales necesarias** página, escriba el Id. de usuario de administrador y la contraseña y haga clic en **Aceptar**.  
  
    6.  En el **configurar el servidor RADIUS** página escriba la misma contraseña usada para el usuario administrador para la **secretos** y **contraseña maestra**. Escriba el Id. de usuario de administrador y la contraseña y haga clic en **configurar**.  
  
    7.  Compruebe que el mensaje **'servidor RADIUS se configuró correctamente'** se muestra. Haga clic en **Listo**. Cerrar la **consola del operador RSA**.  
  
    8.  Vuelva a la **"Consola de seguridad de RSA"** .  
  
    9. En el **RADIUS** ficha en **servidores RADIUS**. Compruebe que aparece ese rsa.corp.contoso.com.  
  
9. Configure el servidor RSA como cliente de autenticación de RSA.  
  
    1.  En el **RADIUS** , haga clic **clientes RADIUS** y **Agregar nuevo**.  
  
    2.  Haga clic en el **cualquier cliente RADIUS** casilla de verificación.  
  
    3.  Escriba una contraseña segura de su elección en el **secreto compartido** campo. Utilizará esta misma contraseña más tarde al configurar EDGE1 para OTP.  
  
    4.  Deje el **dirección IP** campo en blanco y el **marca y modelo** entrada como **RADIUS estándar**.  
  
    5.  Haga clic en **guardar sin agente RSA**.  
  
10. Cree los archivos necesarios para configurar EDGE1 como un agente de autenticación de RSA.  
  
    1.  En el **acceso** , resalte **los agentes de autenticación**y haga clic en **Agregar nuevo**.  
  
    2.  Tipo **EDGE1** en el **Hostname** campo y haga clic en **IP resolver**.  
  
    3.  Tenga en cuenta que la dirección IP de EDGE1 ahora se muestra en el **dirección IP** campo. Haga clic en **Guardar**.  
  
11. Generar un archivo de configuración para el servidor EDGE1 (AM_Config.zip).  
  
    1.  En el **acceso** , resalte **los agentes de autenticación**y haga clic en **generar archivo de configuración**.  
  
    2.  En el **generar archivo de configuración** página haga clic en **generar archivo de configuración**y, a continuación, haga clic en **Descargar ahora**.  
  
    3.  Haga clic en **guardar**, vaya a C:\ Instalación de RSA y haga clic en **guardar**.  
  
    4.  Haga clic en **cerrar** en el **descarga completa** cuadro de diálogo.  
  
12. Generar un archivo de secreto de nodo para el servidor EDGE1 (EDGE1_NodeSecret.zip).  
  
    1.  En el **acceso** , resalte **los agentes de autenticación**y haga clic en **administrar existente**.  
  
    2.  Haga clic en el nodo configurado actual EDGE1 y haga clic en **administrar nodo secreto**.  
  
    3.  Compruebe el **crear un nuevo secreto de nodo aleatorio y exportar el secreto de nodo a un archivo** casilla de verificación.  
  
    4.  Escriba la misma contraseña usada para el usuario administrador en el **contraseña de cifrado** y **Confirmar contraseña de cifrado** campos y haga clic en **guardar**.  
  
    5.  En el **genera archivos de nodo secreto** página haga clic en **Descargar ahora**.  
  
    6.  En el **de descarga del archivo** diálogo haga clic en **guardar**, vaya a la instalación de C:\RSA y haga clic en **guardar**. Haga clic en **cerrar** en el **descarga completa** cuadro de diálogo.  
  
    7.  Desde el RSA Authentication Manager media copia \auth_mgr\windows-x86_64\am\rsa-ace_nsload\win32-5.0-x86\agent_nsload.exe a C:\RSA instalación.  
  
## <a name="BKMK_DAProbeUser"></a>Crear DAProbeUser  
  
1.  En el **consola de RSA Security** haga clic en el **identidad** , haga clic **usuarios**y haga clic en **Agregar nuevo**.  
  
2.  En el **Last Name:** sección tipo **sondeo**y en el **Id. de usuario:** sección tipo **DAProbeUser**. En el **contraseña:** y **Confirmar contraseña:** secciones escriba una contraseña segura. Desactive el **'Que el usuario cambie la contraseña en el siguiente inicio de sesión'** casilla de verificación y haga clic en **guardar**.  
  
## <a name="InstToken"></a>Instalar el token de software de RSA SecurID en CLIENT1  
Utilice este procedimiento para instalar el token de software de SecurID en CLIENT1.  
  
#### <a name="install-securid-software-token"></a>Instalar el token de software de SecurID  
  
1.  En el equipo CLIENT1, cree la carpeta archivos de C:\RSA. Copie el archivo Software_Tokens.zip desde C:\RSA instalación en el equipo RSA en C:\RSA archivos. Extraiga el archivo User1_000031701832.SDTID a archivos C:\RSA en CLIENT1.  
  
2.  Obtener acceso al origen de token media software de RSA SecurID y haga doble clic en RSASECURIDTOKEN410 en el **aplicación de cliente de SecurID SoftwareToken** carpeta para iniciar la instalación de RSA SecurID. Si el **abrir archivo - Advertencia de seguridad** aparece el mensaje, a continuación, haga clic en **ejecutar**.  
  
3.  En el **RSA SecurID Software Token - Asistente InstallShield** diálogo haga clic en **siguiente** dos veces.  
  
4.  Acepte el contrato de licencia y haga clic en **siguiente**.  
  
5.  En el **el tipo de instalación** cuadro de diálogo Seleccionar **típica**, haga clic en **siguiente**y haga clic en **instalar**.  
  
6.  Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
7.  Seleccione el **iniciar RSA SecurID Software Token** casilla de verificación y haga clic en **finalizar**.  
  
8.  Haga clic en **importar desde el archivo**.  
  
9. Haga clic en **examinar**, seleccione C:\RSA Files\User1_000031701832.SDTID y haga clic en **abierto**.  
  
10. Haga clic en **Aceptar** dos veces.  
  
## <a name="configAuthAgt"></a>Configurar EDGE1 como un agente de autenticación de RSA  
Use este procedimiento para configurar EDGE1 para realizar la autenticación de RSA.  
  
#### <a name="configure-the-rsa-authentication-agent"></a>Configurar al agente de autenticación de RSA  
  
1. En EDGE1 abra el Explorador de Windows y cree la carpeta archivos de C:\RSA. Vaya a los medios de instalación de RSA ACE.  
  
2. Copia los archivos agent_nsload.exe, AM_Config.zip y EDGE1_NodeSecret.zip desde el medio de RSA para C:\RSA archivos.  
  
3. Extraiga el contenido de ambos archivos zip en las ubicaciones siguientes:  
  
   1.  C:\Windows\system32\  
  
   2.  C:\Windows\SysWOW64\  
  
4. Copiar agent_nsload.exe a C:\Windows\SysWOW64\\.  
  
5. Abra un símbolo del sistema con privilegios elevados y vaya a C:\Windows\SysWOW64.  
  
6. Tipo **agent_nsload.exe -f nodesecret.rec -p <password>**  donde <password> es la contraseña segura que creó durante la configuración inicial de RSA. Presiona Entrar.  
  
7. Copie C:\Windows\SysWOW64\securid en C:\Windows\System32.  
  
## <a name="configOTP"></a>Configurar EDGE1 para admitir la autenticación de OTP  
Utilice este procedimiento para configurar OTP de DirectAccess y compruebe la configuración.  
  
#### <a name="configure-otp-for-directaccess"></a>Configurar la OTP de DirectAccess  
  
1.  En EDGE1, abra el administrador del servidor y haga clic en **acceso remoto** en el panel izquierdo.  
  
2.  Haga clic en **EDGE1** en el panel de servidores y seleccione **administración de acceso remoto**.  
  
3.  Haga clic en **configuración**.  
  
4.  En el **instalación de DirectAccess** ventana, en **paso 2: servidor de acceso remoto**, haga clic en **editar**.  
  
5.  Haga clic en **siguiente** tres veces y, en el **autenticación** sección Seleccione **autenticación en dos fases** y **usar OTP**y asegúrese de que **Usar certificados de equipo** está activada. Compruebe que la CA raíz está establecida en **CN = corp-APP1-CA**. Haz clic en **Siguiente**.  
  
6.  En el **servidor RADIUS de OTP** sección, haga doble clic en el espacio en blanco **nombre del servidor** campo.  
  
7.  En el **agregar un servidor RADIUS** cuadro de diálogo, escriba **RSA** en el **nombre del servidor** campo. Haga clic en **cambio** junto a la **secreto compartido** campo y escriba la misma contraseña que usó al configurar los clientes RADIUS en el servidor RSA en el **secreto nuevo** y  **Confirmar secreto nuevo** campos. Haga clic en **Aceptar** dos veces y haga clic en **siguiente**.  
  
    > [!NOTE]  
    > Si el servidor RADIUS está en un dominio diferente del servidor de acceso remoto, el **nombre del servidor** campo debe especificar el FQDN del servidor RADIUS.  
  
8.  En el **servidores de CA OTP** sección APP1.corp.contoso.com seleccione y haga clic en **agregar**. Haz clic en **Siguiente**.  
  
9. En el **plantillas de certificado de OTP** página haga clic en **examinar** para seleccionar una plantilla de certificado que se usa para la inscripción de certificados emitidos para la autenticación OTP y, en el  **Plantillas de certificado** cuadro de diálogo seleccione **DAOTPLogon**. Haga clic en **Aceptar**. Haga clic en **examinar** para seleccionar una plantilla de certificado que se usa para inscribir el certificado utilizado por el servidor de acceso remoto a las solicitudes de inscripción de certificado OTP de inicio de sesión y, en el **plantillas de certificado** cuadro de diálogo Seleccione **DAOTPRA**. Haga clic en **Aceptar**. Haz clic en **Siguiente**.  
  
10. En el **instalación del servidor de acceso remoto** página haga clic en **finalizar**y haga clic en **finalizar** en el **Asistente de DirectAccess experto**.  
  
11. En el **revisión de acceso remoto** haga clic en el cuadro de diálogo **aplicar**, espere para que se puede actualizar la directiva de DirectAccess y haga clic en **cerrar**.  
  
12. En el **iniciar** , escriba**powershell.exe**, haga clic en **powershell**, haga clic en **avanzadas**y haga clic en **ejecutar como administrador**. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
13. En la ventana de Windows PowerShell, escriba **gpupdate /force** y presione ENTRAR.  
  
14. Cierre y vuelva a abrir la consola de administración de acceso remoto y compruebe que toda la configuración de OTP es correcta.  
  



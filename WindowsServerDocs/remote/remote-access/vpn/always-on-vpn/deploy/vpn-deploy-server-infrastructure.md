---
title: Configurar la infraestructura de servidor
description: En este paso, instale y configure los componentes de servidor necesarios para admitir la VPN. Los componentes de servidor incluyen la configuración de PKI para distribuir los certificados usados por los usuarios, el servidor VPN y el servidor NPS.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.reviewer: deverette
ms.openlocfilehash: 8a07d84427b770da465b8712d71eb846a7c9cf8d
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749604"
---
# <a name="step-2-configure-the-server-infrastructure"></a>Paso 2. Configurar la infraestructura de servidor

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Paso 1. Planear la implementación de VPN de Always On](always-on-vpn-deploy-planning.md)
- [**Siguiente:** Paso 3. Configurar el servidor de acceso remoto para VPN de Always On](vpn-deploy-ras.md)

En este paso, deberá instalar y configurar los componentes de servidor necesarios para admitir la VPN. Los componentes de servidor incluyen la configuración de PKI para distribuir los certificados usados por los usuarios, el servidor VPN y el servidor NPS.  También configurar RRAS para admitir las conexiones IKEv2 y el servidor NPS para realizar la autorización para las conexiones VPN.

## <a name="configure-certificate-autoenrollment-in-group-policy"></a>Configurar la inscripción automática de certificados en la directiva de grupo
En este procedimiento, configurar Directiva de grupo en el controlador de dominio para que los miembros del dominio solicitar automáticamente certificados de usuario y equipo. Esto permite que los usuarios VPN solicitar y recuperar certificados de usuario que se autentican automáticamente las conexiones VPN. Del mismo modo, esta directiva permite servidores NPS para solicitar el servidor de certificados de autenticación automáticamente. 

Inscribe manualmente certificados en servidores VPN.

>[!TIP]
>Para los equipos combinados que no son dominio, consulte [equipos unidos a un configuración de CA para que no sea de dominio](#ca-configuration-for-non-domain-joined-computers). Puesto que el servidor RRAS no está unido al dominio, la inscripción automática no se puede usar para inscribir el certificado de puerta de enlace VPN.  Por lo tanto, utilizar un procedimiento de solicitud de certificados sin conexión.

1. En un controlador de dominio, abra Administración de directivas de grupo.

2. En el panel de navegación, haga clic en el dominio (por ejemplo, corp.contoso.com) y luego seleccione **crear un GPO en este dominio y vincularlo aquí**.

3. En el cuadro de diálogo nuevo GPO, escriba **directiva de inscripción automática**, a continuación, seleccione **Aceptar**.

4. En el panel de navegación, haga clic en **directiva de inscripción automática**, a continuación, seleccione **editar**.

5. En el Editor de administración de directivas de grupo, complete los pasos siguientes para configurar la inscripción automática de certificados de equipo:

    1. En el panel de navegación, vaya a **configuración del equipo** > **directivas** > **Windows configuración**  >   **Configuración de seguridad** > **directivas de clave pública**.

    2. En el panel de detalles, haga clic en **cliente de servicios de certificados-inscripción automática**, a continuación, seleccione **propiedades**.

    3. En el cliente de servicios de certificados-inscripción automática cuadro de diálogo Propiedades, en **modelo de configuración**, seleccione **habilitado**.

    4. Seleccione **Renovar certificados expirados, actualizar certificados pendientes y quitar certificados revocados** y **Actualizar certificados que usan plantillas de certificado**.

    5. Seleccione **Aceptar**.

6. En el Editor de administración de directivas de grupo, complete los pasos siguientes para configurar inscripción automática de certificados:

    1. En el panel de navegación, vaya a **configuración de usuario** > **directivas** > **Windows configuración**  >   **Configuración de seguridad** > **directivas de clave pública**.

    2. En el panel de detalles, haz clic con el botón secundario en **Cliente de Servicios de certificados - Inscripción automática** y selecciona **Propiedades**.

    3. En el cliente de servicios de certificados-inscripción automática cuadro de diálogo Propiedades, en **modelo de configuración**, seleccione **habilitado**.

    4. Seleccione **Renovar certificados expirados, actualizar certificados pendientes y quitar certificados revocados** y **Actualizar certificados que usan plantillas de certificado**.

    5. Seleccione **Aceptar**.

    6. Cierra el Editor de administración de directivas de grupo.

7. Cierre Administración de directivas de grupo.

### <a name="ca-configuration-for-non-domain-joined-computers"></a>Configuración de entidad emisora de certificados para equipos combinados que no sea de dominio

Puesto que el servidor RRAS no está unido al dominio, la inscripción automática no se puede usar para inscribir el certificado de puerta de enlace VPN.  Por lo tanto, utilizar un procedimiento de solicitud de certificados sin conexión.

1. En el servidor RRAS, generar un archivo denominado **VPNGateway.inf** en función de la solicitud de directiva de certificado de ejemplo proporcionada en el apéndice (sección 0) y personalizar las siguientes entradas:

   - En la sección [NewRequest], reemplazar vpn.contoso.com utilizado para el nombre de asunto con el elegido [_cliente_] FQDN del punto de conexión de VPN.

   - En la sección [Extensions], reemplazar vpn.contoso.com usa el nombre de sujeto alternativo con el elegido [_cliente_] FQDN del punto de conexión de VPN.

2. Guardar o copiar el **VPNGateway.inf** archivo en una ubicación especificada.

3. Desde un símbolo del sistema con privilegios elevados, navegue hasta la carpeta que contiene el **VPNGateway.inf** archivo y escriba:

   ```
   certreq -new VPNGateway.inf VPNGateway.req
   ```

4. Copie el recién creado **VPNGateway.req** archivo de salida a un servidor de la entidad de certificación o estación de trabajo de acceso con privilegios (PAW).

5. Guardar o copiar el **VPNGateway.req** archivo en una ubicación especificada en el servidor de la entidad de certificación, o la estación de trabajo de acceso con privilegios (PAW).

6. Desde un símbolo del sistema con privilegios elevados, navegue hasta la carpeta que contiene el archivo VPNGateway.req creado en el paso anterior y escriba:

   ```
   certreq -attrib “CertificateTemplate:[Customer]VPNGateway” -submit VPNgateway.req VPNgateway.cer
   ```

7. Si se lo solicite la ventana Lista de la entidad de certificación, seleccione la CA de empresa apropiada para atender la solicitud de certificado.

8. Copie el recién creado **VPNGateway.cer** archivo de salida en el servidor RRAS. 

9. Guardar o copiar el **VPNGateway.cer** archivo en una ubicación especificada en el servidor RRAS.

10. Desde un símbolo del sistema con privilegios elevados, navegue hasta la carpeta que contiene el archivo VPNGateway.cer creado en el paso anterior y escriba:

    ```
    certreq -accept VPNGateway.cer
    ```

11. Ejecute el complemento MMC certificados, como se describe [aquí](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in) seleccionando el **cuenta de equipo** opción.

12. Asegúrese de que existe un certificado válido para el servidor RRAS con las siguientes propiedades:

    - **Fines previstos:** Autenticación de servidor, seguridad IP IKE intermedia 

    - **Plantilla de certificado:** [_cliente_] servidor VPN

#### <a name="example-vpngatewayinf-script"></a>Por ejemplo: Secuencia de comandos VPNGateway.inf

Aquí puede ver un script de ejemplo de una directiva de solicitud de certificado que usa para solicitar un certificado de puerta de enlace VPN mediante un proceso fuera de banda.

>[!TIP]
>Puede encontrar una copia de la secuencia de comandos VPNGateway.inf en el Kit IP ofrece VPN en la carpeta de directivas de solicitud de certificado. Actualizar solo el 'Asunto' y '\_continuar\_' con valores específicos del cliente.

```
[Version] 

Signature="$Windows NT$"

[NewRequest]
Subject = "CN=vpn.contoso.com"
Exportable = FALSE   
KeyLength = 2048     
KeySpec = 1          
KeyUsage = 0xA0      
MachineKeySet = True
ProviderName = "Microsoft RSA SChannel Cryptographic Provider"
RequestType = PKCS10 

[Extensions]
2.5.29.17 = "{text}"
_continue_ = "dns=vpn.contoso.com&"
```

## <a name="create-the-vpn-users-vpn-servers-and-nps-servers-groups"></a>Crear los usuarios de VPN, servidores VPN y grupos de servidores NPS

En este procedimiento, puede agregar un nuevo grupo de Active Directory (AD) que contenga los usuarios con permiso para usar la VPN para conectarse a la red de su organización.

Este grupo tiene dos propósitos:

- Define qué usuarios tienen permiso para la inscripción automática para los certificados de usuario que requiere la VPN.

- Define los usuarios que autoriza el NPS para acceso VPN.

Mediante el uso de un grupo personalizado, si desea revocar el acceso VPN de un usuario, puede quitar ese usuario del grupo.

También agrega un grupo que contiene los servidores VPN y otro grupo que contiene los servidores NPS. Use estos grupos para restringir las solicitudes de certificado a sus miembros.

>[!NOTE]
>Se recomienda VPN servidores que residen en el perímetro de DMA no estar unido al dominio. Sin embargo, si prefiere que los servidores VPN unido al dominio para una administración mejorada (directivas de grupo, el agente de copia de seguridad y supervisión, ningún usuario local para administrar y así sucesivamente), a continuación, agregar un grupo de AD a la plantilla de certificado de servidor VPN.

### <a name="configure-the-vpn-users-group"></a>Configurar el grupo de usuarios de VPN

1. En un controlador de dominio, abra usuarios y equipos de usuarios de Active Directory.

2. Haga clic en un contenedor o unidad organizativa, seleccione **New**, a continuación, seleccione **grupo**.

3. En **conmutaciónporerror**, escriba **usuarios de VPN**, a continuación, seleccione **Aceptar**.

4. Haga clic en **usuarios de VPN** y seleccione **propiedades**.

5. En el **miembros** ficha del cuadro de diálogo Propiedades de los usuarios de VPN, seleccione **agregar**.

6. En el cuadro de diálogo Seleccionar usuarios, agregar todos los usuarios que necesitan acceso a la VPN y seleccione **Aceptar**.

7. Cierre Usuarios y equipos de Active Directory.

### <a name="configure-the-vpn-servers-and-nps-servers-groups"></a>Configurar los grupos de servidores VPN y servidores NPS

1. En un controlador de dominio, abra usuarios y equipos de usuarios de Active Directory.

2. Haga clic en un contenedor o unidad organizativa, seleccione **New**, a continuación, seleccione **grupo**.

3. En **conmutaciónporerror**, escriba **servidores VPN**, a continuación, seleccione **Aceptar**.

4. Haga clic en **servidores VPN** y seleccione **propiedades**.

5. En el **miembros** ficha del cuadro de diálogo Propiedades de los servidores VPN, seleccione **agregar**.

6. Seleccione **tipos de objeto**, seleccione el **equipos** casilla y luego seleccione **Aceptar**.

7. En **escriba los nombres de objeto para seleccionar**, escriba los nombres de los servidores VPN y luego seleccione **Aceptar**.

8. Seleccione **Aceptar** para cerrar el cuadro de diálogo Propiedades del servidor VPN.

9. Repita los pasos anteriores para el grupo de servidores NPS.

10. Cierre Usuarios y equipos de Active Directory.

## <a name="create-the-user-authentication-template"></a>Crear la plantilla de autenticación de usuario

En este procedimiento, configure una plantilla de autenticación de cliente / servidor personalizadas. Esta plantilla es necesaria porque desea mejorar la seguridad general del certificado mediante la selección de niveles de compatibilidad actualizada y elegir el proveedor criptográfico de plataforma de Microsoft. Este último cambio le permite usar el TPM en los equipos cliente para proteger el certificado. Para obtener información general de TPM, consulte [Trusted Platform Module Technology Overview](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview).

**Procedimiento:**

1. En la CA, abra la entidad de certificación.

2. En el panel de navegación, haga clic en **plantillas de certificado** y seleccione **administrar**.

3. En la consola de plantillas de certificado, haga clic en **usuario** y seleccione **Duplicar plantilla**.

   >[!WARNING]
   >No seleccione **aplicar** o **Aceptar** en cualquier momento antes del paso 10.  Si selecciona estos botones antes de entrar en todos los parámetros, muchas opciones se fija y ya no es editable. Por ejemplo, en el **criptografía** pestaña si _proveedor de almacenamiento criptográfico heredado_ se muestra en el campo de categoría del proveedor, se deshabilita, ninguna prevención del cambio. La única alternativa consiste en eliminar la plantilla y volver a crearla.  

4. En el cuadro de diálogo Propiedades de plantilla nueva, en el **General** ficha, realice los pasos siguientes:

   1. En **nombre para mostrar plantilla**, tipo **autenticación de usuario de VPN**.

   2. Desactive el **Publicar certificado en Active Directory** casilla de verificación.

5. En el **seguridad** ficha, realice los pasos siguientes:

   1. Seleccione **agregar**.

   2. En Seleccionar usuarios, equipos, cuentas de servicio o cuadro de diálogo grupos, escriba **usuarios de VPN**, a continuación, seleccione **Aceptar**.

   3. En **los nombres de usuario o grupo**, seleccione **usuarios de VPN**.

   4. En **permisos para los usuarios de VPN**, seleccione el **inscribir** y **inscripción automática** casillas de verificación en la **permitir** columna.

      >[!TIP]
      >Asegúrese de mantener activada la casilla de verificación de lectura. En otras palabras, necesita los permisos de lectura para la inscripción. 

   5. En **los nombres de usuario o grupo**, seleccione **los usuarios del dominio**, a continuación, seleccione **quitar**.

6. En el **compatibilidad** ficha, realice los pasos siguientes:

   1. En **entidad de certificación**, seleccione **Windows Server 2012 R2**.

   2. En el **cambios resultantes** cuadro de diálogo, seleccione **Aceptar**.

   3. En **destinatario del certificado**, seleccione **Windows 8.1 / Windows Server 2012 R2**.

   4. En el **cambios resultantes** cuadro de diálogo, seleccione **Aceptar**.

7. En el **tratamiento de la solicitud** ficha, desactive la **permitir que la clave privada se pueda exportar** casilla de verificación.

8. En el **criptografía** ficha, realice los pasos siguientes:

   1. En **categoría del proveedor**, seleccione **Key Storage Provider**.

   2. Seleccione **las solicitudes deben usar uno de los siguientes proveedores**.

   3. Seleccione el **proveedor criptográfico de plataforma de Microsoft** casilla de verificación.

9. En el **nombre de sujeto** ficha, si no tiene una dirección de correo electrónico que aparece en todas las cuentas de usuario, desactive la **incluir el nombre de correo electrónico en nombre de sujeto** y **nombre de correo electrónico** casillas de verificación.

10. Seleccione **Aceptar** para guardar la plantilla de certificado de autenticación de usuarios de VPN.

11. Cierre la consola Plantillas de certificado.

12. En el panel de navegación del complemento entidad de certificación, haga clic en **plantillas de certificado**, seleccione **New** y, a continuación, seleccione **plantilla de certificado para emitir**.

13. Seleccione **autenticación de usuario de VPN**, a continuación, seleccione **Aceptar**.

14. Cierre el complemento entidad de certificación.

## <a name="create-the-vpn-server-authentication-template"></a>Crear la plantilla de autenticación de servidor VPN

En este procedimiento, puede configurar una nueva plantilla de autenticación de servidor para el servidor VPN. Adición de que la directiva de seguridad IP (IPsec) IKE Intermediate aplicación permite al servidor filtro certificados si hay más de un certificado con la autenticación de servidor de uso mejorado de clave.

>[!IMPORTANT]
>Dado que los clientes VPN de acceso a este servidor desde Internet público, el asunto y nombres alternativos son distintos del nombre interno del servidor. Como resultado, no puede inscribir automáticamente este certificado en servidores VPN.

**Requisitos previos:**

Servidores VPN unido al dominio

**Procedimiento:**

1. En la CA, abra la entidad de certificación.

2. En el panel de navegación, haga clic en **plantillas de certificado** y seleccione **administrar**.

3. En la consola de plantillas de certificado, haga clic en **servidor RAS e IAS** y seleccione **Duplicar plantilla**.

4. En el cuadro de diálogo Propiedades de plantilla nueva, en el **General** ficha **nombre para mostrar plantilla**, escriba un nombre descriptivo para el servidor VPN, por ejemplo, **deautenticacióndeservidordeVPN** o **servidor RADIUS**.

5. En el **extensiones** ficha, realice los pasos siguientes:

    1. Seleccione **las directivas de aplicación**, a continuación, seleccione **editar**.

    2. En el **Editar extensión de directivas de aplicación** cuadro de diálogo, seleccione **agregar**.

    3. En el **agregar directivas de aplicación** cuadro de diálogo, seleccione **seguridad IP IKE intermedia**, a continuación, seleccione **Aceptar**.
   
        Agregando la IP IKE intermedia para el EKU de seguridad ayuda a en escenarios donde existe más de un certificado de autenticación de servidor en el servidor VPN. Cuando está presente la seguridad IP IKE intermedia, IPSec usa solo el certificado con las opciones de EKU. Sin esto, podría producirse un error de autenticación de IKEv2 con Error 13801: Las credenciales de autenticación de IKE son inaceptables.

    4. Seleccione **Aceptar** para volver a la **propiedades de plantilla nueva** cuadro de diálogo.

6. En el **seguridad** ficha, realice los pasos siguientes:

    1. Seleccione **agregar**.

    2. En el **Seleccionar usuarios, equipos, cuentas de servicio o grupos** diálogo cuadro, escriba **servidores VPN**, a continuación, seleccione **Aceptar**.

    3. En **los nombres de usuario o grupo**, seleccione **servidores VPN**.

    4. En **permisos para los servidores VPN**, seleccione el **inscribir** casilla de verificación en la **permitir** columna.

    5. En **los nombres de usuario o grupo**, seleccione **servidores RAS e IAS**, a continuación, seleccione **quitar**.

7. En el **nombre de sujeto** ficha, realice los pasos siguientes:

    1. Seleccione **proporcionado por el solicitante**.

    2. En el **plantillas de certificado** el cuadro de diálogo de advertencia, seleccione **Aceptar**.

8. (Opcional) Si va a configurar el acceso condicional para la conectividad VPN, seleccione la **tratamiento de la solicitud** y, después, seleccione **permitir que la clave privada se pueda exportar**.

9. Seleccione **Aceptar** para guardar la plantilla de certificado de servidor VPN.

10. Cierre la consola Plantillas de certificado.

11. En el panel de navegación del complemento entidad de certificación, haga clic en **plantillas de certificado**, seleccione **New** y, a continuación, seleccione **plantilla de certificado para emitir**.

12. Seleccione el nombre que eligió en el paso 4 anterior y haga clic en **Aceptar**.

13. Cierre el complemento entidad de certificación.

## <a name="create-the-nps-server-authentication-template"></a>Crear la plantilla de autenticación de servidor NPS

Cree la plantilla de certificado de la tercera y última es la plantilla de autenticación del servidor NPS. La plantilla de autenticación del servidor NPS es una simple copia de la plantilla de servidor RAS e IAS protegida al grupo de servidores de NPS que creó anteriormente en esta sección.

Va a configurar este certificado para la inscripción automática.

**Procedimiento:**

1. En la CA, abra la entidad de certificación.

2. En el panel de navegación, haga clic en **plantillas de certificado** y seleccione **administrar**.

3. En la consola de plantillas de certificado, haga clic en **servidor RAS e IAS**y seleccione **Duplicar plantilla**.

4. En el cuadro de diálogo Propiedades de plantilla nueva, en el **General** ficha **nombre para mostrar plantilla**, tipo **autenticación del servidor NPS**.

5. En el **seguridad** ficha, realice los pasos siguientes:

    1. Seleccione **agregar**.

    2. En el **Seleccionar usuarios, equipos, cuentas de servicio o grupos** diálogo cuadro, escriba **servidores NPS**, a continuación, seleccione **Aceptar**.

    3. En **los nombres de usuario o grupo**, seleccione **servidores NPS**.

    4. En **permisos para los servidores NPS**, seleccione el **inscribir** y **inscripción automática** casillas de verificación en la **permitir** columna.

    5. En **los nombres de usuario o grupo**, seleccione **servidores RAS e IAS**, a continuación, seleccione **quitar**.

6. Seleccione **Aceptar** para guardar la plantilla de certificado de servidor NPS.

7. Cierre la consola Plantillas de certificado.

8. En el panel de navegación del complemento entidad de certificación, haga clic en **plantillas de certificado**, seleccione **New** y, a continuación, seleccione **plantilla de certificado para emitir**.

9. Seleccione **autenticación del servidor NPS**y seleccione **Aceptar**.

10. Cierre el complemento entidad de certificación.

## <a name="enroll-and-validate-the-user-certificate"></a>Inscribir y validar el certificado de usuario

Dado que usa directiva de grupo para inscribir automáticamente certificados de usuario, solo se necesita actualizar la directiva y Windows 10 se inscribirá automáticamente la cuenta de usuario para el certificado correcto. A continuación, puede validar el certificado en la consola de certificados.

**Procedimiento:**

1. Inicie sesión en un equipo cliente unido al dominio como un miembro de la **usuarios de VPN** grupo.

2. Presione la tecla Windows + R, escriba **gpupdate /force**, y presione ENTRAR.

3. En el menú Inicio, escriba **certmgr.msc**, y presione ENTRAR.

4. En el complemento certificados, en **Personal**, seleccione **certificados**. Los certificados aparecen en el panel de detalles.

5. Haga clic en el certificado que tenga el nombre de usuario de dominio actual y, a continuación, seleccione **abierto**.

6. En el **General** , confirme que la fecha aparece en **válido desde** es la fecha de hoy. Si no lo está, es posible que ha seleccionado el certificado incorrecto.

7. Seleccione **Aceptar**y cierre el complemento certificados.

## <a name="enroll-and-validate-the-server-certificates"></a>Inscribir y validar los certificados de servidor

A diferencia de los certificados de usuario, debe inscribir manualmente certificados del servidor VPN. Una vez que haya inscrito, validar utilizando el mismo proceso que usó para el certificado de usuario. Al igual que el certificado de usuario, el servidor NPS inscribe automáticamente su certificado de autenticación, por lo que todo lo que necesita hacer es validarla.

>[!NOTE]
>Es posible que deba reiniciar los servidores VPN y NPS para que puedan actualizar su pertenencia a grupos para poder completar estos pasos.

### <a name="enroll-and-validate-the-vpn-server-certificate"></a>Inscribir y validar el certificado del servidor VPN

1. En el menú de inicio del servidor VPN, escriba **certlm.msc**, y presione ENTRAR.

2. Haga clic en **Personal**, seleccione **todas las tareas** y, a continuación, seleccione **solicitar un nuevo certificado** para iniciar el Asistente para inscripción de certificados.

3. En la página antes de comenzar, seleccione **siguiente**.

4. En la página Seleccionar directiva de inscripción de certificados, seleccione **siguiente**.

5. En la página de solicitud de certificados, seleccione la casilla de verificación junto al servidor VPN para seleccionarlo.

6. En la casilla de verificación del servidor VPN, seleccione **se necesita más información** para abrir el cuadro de diálogo Propiedades del certificado y complete los pasos siguientes:

    1. Seleccione el **asunto** ficha, seleccione **nombre común** en **nombre de sujeto**, en **tipo**.

    2. En **nombre de sujeto**, en **valor**, escriba el nombre de los clientes de dominio externo que se usa para conectarse a la VPN, por ejemplo, vpn.contoso.com, a continuación, seleccione **agregar**.

    3. En **nombre alternativo**, en **tipo**, seleccione **DNS**.

    4. En **nombre alternativo**, en **valor**, escriba todos los nombres de servidor, los clientes usan para conectarse a la VPN, por ejemplo, vpn.contoso.com, vpn, 132.64.86.2.

    5. Seleccione **agregar** después de escribir cada nombre.

    6. Seleccione **Aceptar** cuando termine.

7. Seleccione **inscribir**.

8. Seleccione **finalizar**.

9. En el complemento certificados, en **Personal**, seleccione **certificados**.
    
    Los certificados de la lista aparecen en el panel de detalles.

10. Haga clic en el certificado con el servidor VPN de nombre y, a continuación, seleccione **abierto**.

11. En el **General** , confirme que la fecha aparece en **válido desde** es la fecha de hoy. Si no lo está, es posible que ha seleccionado un certificado incorrecto.

12. En el **detalles** ficha, seleccione **uso mejorado de clave**y compruebe que **seguridad IP IKE intermedia** y **autenticación de servidor** mostrar en la lista.

13. Seleccione **Aceptar** para cerrar el certificado.

14. Cierre el complemento certificados.

### <a name="validate-the-nps-server-certificate"></a>Validar el certificado de servidor NPS

1. Reinicie el servidor NPS.

2. En el menú de inicio del servidor NPS, escriba **certlm.msc**, y presione ENTRAR.

3. En el complemento certificados, en **Personal**, seleccione **certificados**.

    Los certificados de la lista aparecen en el panel de detalles.

4. Haga clic en el certificado que tiene el servidor NPS asigne un nombre y, a continuación, seleccione **abierto**.

5. En el **General** , confirme que la fecha aparece en **válido desde** es la fecha de hoy. Si no lo está, es posible que ha seleccionado un certificado incorrecto.

6. Seleccione **Aceptar** para cerrar el certificado.

7. Cierre el complemento certificados.

## <a name="next-steps"></a>Pasos siguientes

[Paso 3. Configurar el servidor de acceso remoto para siempre en VPN](vpn-deploy-ras.md): En este paso, configure una VPN de acceso remoto para permitir conexiones VPN de IKEv2, denegar las conexiones desde otros protocolos VPN y asignar un grupo de direcciones IP estáticas para la emisión de direcciones IP a la conexión de los clientes VPN autorizados.

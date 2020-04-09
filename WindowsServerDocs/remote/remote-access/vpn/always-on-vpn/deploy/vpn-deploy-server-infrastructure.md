---
title: Configurar la infraestructura de servidor
description: En este paso, instalará y configurará los componentes del lado servidor necesarios para admitir la VPN. Los componentes del lado servidor incluyen la configuración de la PKI para distribuir los certificados usados por los usuarios, el servidor VPN y el servidor NPS.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.localizationpriority: medium
ms.author: v-tea
author: Teresa-MOTIV
ms.date: 08/30/2018
ms.reviewer: deverette
ms.openlocfilehash: 7c09ae7a792030152780ce4eb0029cea3ca234d2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80818928"
---
# <a name="step-2-configure-the-server-infrastructure"></a>Paso 2. Configurar la infraestructura de servidor

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Paso 1. Planear la Always On la implementación de VPN](always-on-vpn-deploy-planning.md)
- [**Siguiente:** Paso 3. Configurar el servidor de acceso remoto para Always On VPN](vpn-deploy-ras.md)

En este paso, instalará y configurará los componentes del lado servidor necesarios para admitir la VPN. Los componentes del lado servidor incluyen la configuración de la PKI para distribuir los certificados usados por los usuarios, el servidor VPN y el servidor NPS.  También configura RRAS para admitir conexiones IKEv2 y el servidor NPS para realizar la autorización para las conexiones VPN.

## <a name="configure-certificate-autoenrollment-in-group-policy"></a>Configuración de la inscripción automática de certificados en directiva de grupo
En este procedimiento, configurará directiva de grupo en el controlador de dominio para que los miembros del dominio soliciten automáticamente certificados de usuario y de equipo. Esto permite a los usuarios de VPN solicitar y recuperar certificados de usuario que autentican conexiones VPN automáticamente. Del mismo modo, esta directiva permite que los servidores NPS soliciten automáticamente los certificados de autenticación del servidor. 

Los certificados se inscriben manualmente en los servidores VPN.

>[!TIP]
>Para equipos Unidos que no estén en el dominio, consulte [configuración de CA para equipos no Unidos a un dominio](#ca-configuration-for-non-domain-joined-computers). Puesto que el servidor RRAS no está unido a un dominio, no se puede usar la inscripción automática para inscribir el certificado de puerta de enlace VPN.  Por lo tanto, use un procedimiento de solicitud de certificado sin conexión.

1. En un controlador de dominio, abra Administración de directiva de grupo.

2. En el panel de navegación, haga clic con el botón secundario en el dominio (por ejemplo, corp.contoso.com), seleccione **crear un GPO en este dominio y vincúlelo aquí**.

3. En el cuadro de diálogo nuevo GPO, escriba **Directiva de inscripción automática**y, a continuación, haga clic en **Aceptar**.

4. En el panel de navegación, haga clic con el botón derecho en **Directiva de inscripción automática**y seleccione **Editar**.

5. En el Editor de administración de directivas de grupo, complete los pasos siguientes para configurar la inscripción automática de certificados de equipo:

    1. En el panel de navegación, vaya a **configuración del equipo** > **directivas** > **configuración de Windows** > configuración de **seguridad** > **directivas de clave pública**.

    2. En el panel de detalles, haga clic con el botón secundario en **cliente de servicios de Certificate Server-inscripción automática**y, a continuación, seleccione **propiedades**.

    3. En el cuadro de diálogo cliente de servicios de Certificate Server: propiedades de inscripción automática, en **modelo de configuración**, seleccione **habilitado**.

    4. Seleccione **Renovar certificados expirados, actualizar certificados pendientes y quitar certificados revocados** y **Actualizar certificados que usan plantillas de certificado**.

    5. Seleccione **Aceptar**.

6. En el Editor de administración de directivas de grupo, complete los pasos siguientes para configurar la inscripción automática de certificados de usuario:

    1. En el panel de navegación, vaya a **configuración de usuario** > **directivas** > **configuración de Windows** > configuración de **seguridad** > **directivas de clave pública**.

    2. En el panel de detalles, haz clic con el botón secundario en **Cliente de Servicios de certificados - Inscripción automática** y selecciona **Propiedades**.

    3. En el cuadro de diálogo cliente de servicios de Certificate Server: propiedades de inscripción automática, en **modelo de configuración**, seleccione **habilitado**.

    4. Seleccione **Renovar certificados expirados, actualizar certificados pendientes y quitar certificados revocados** y **Actualizar certificados que usan plantillas de certificado**.

    5. Seleccione **Aceptar**.

    6. Cierra el Editor de administración de directivas de grupo.

7. Cierre Administración de directivas de grupo.

### <a name="ca-configuration-for-non-domain-joined-computers"></a>Configuración de CA para equipos no Unidos a un dominio

Puesto que el servidor RRAS no está unido a un dominio, no se puede usar la inscripción automática para inscribir el certificado de puerta de enlace VPN.  Por lo tanto, use un procedimiento de solicitud de certificado sin conexión.

1. En el servidor RRAS, genere un archivo llamado **VPNGateway. inf** basado en la solicitud de directiva de certificado de ejemplo proporcionada en el apéndice a (sección 0) y Personalice las siguientes entradas:

   - En la sección [NewRequest], reemplace vpn.contoso.com usado por el nombre del firmante por el FQDN del punto de conexión de VPN [_Customer_] elegido.

   - En la sección [extensions], reemplace vpn.contoso.com usado por el nombre alternativo del firmante por el FQDN del punto de conexión de VPN [_Customer_] elegido.

2. Guarde o copie el archivo **VPNGateway. inf** en una ubicación seleccionada.

3. Desde un símbolo del sistema con privilegios elevados, navegue hasta la carpeta que contiene el archivo **VPNGateway. inf** y escriba:

   ```
   certreq -new VPNGateway.inf VPNGateway.req
   ```

4. Copie el archivo de salida de **VPNGateway. req** recién creado en un servidor de entidad de certificación o en una estación de trabajo de acceso con privilegios (pata).

5. Guarde o copie el archivo **VPNGateway. req** en una ubicación seleccionada en el servidor de la entidad de certificación o en la estación de trabajo de acceso con privilegios (pata).

6. Desde un símbolo del sistema con privilegios elevados, navegue hasta la carpeta que contiene el archivo VPNGateway. req creado en el paso anterior y escriba:

   ```
   certreq -attrib "CertificateTemplate:[Customer]VPNGateway" -submit VPNgateway.req VPNgateway.cer
   ```

7. Si se le solicita la ventana Lista de entidades de certificación, seleccione la CA de empresa adecuada para dar servicio a la solicitud de certificado.

8. Copie el archivo de salida **VPNGateway. cer** recién creado en el servidor RRAS. 

9. Guarde o copie el archivo **VPNGateway. cer** en una ubicación seleccionada en el servidor RRAS.

10. Desde un símbolo del sistema con privilegios elevados, navegue hasta la carpeta que contiene el archivo VPNGateway. cer creado en el paso anterior y escriba:

    ```
    certreq -accept VPNGateway.cer
    ```

11. Ejecute el complemento MMC certificados tal y como se describe [aquí](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in) selección de la opción **cuenta de equipo** .

12. Asegúrese de que existe un certificado válido para el servidor RRAS con las siguientes propiedades:

    - **Propósitos planteados:** Autenticación de servidor, seguridad IP IKE intermedia 

    - **Plantilla de certificado:** [_cliente_] servidor VPN

#### <a name="example-vpngatewayinf-script"></a>Ejemplo: VPNGateway. inf script

Aquí puede ver un script de ejemplo de una directiva de solicitud de certificado que se usa para solicitar un certificado de puerta de enlace de VPN mediante un proceso fuera de banda.

>[!TIP]
>Puede encontrar una copia del script VPNGateway. inf en el kit de direcciones IP de la oferta de VPN, en la carpeta de directivas de solicitud de certificado. Actualice solo ' subject ' y '\_continue\_' con valores específicos del cliente.

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

## <a name="create-the-vpn-users-vpn-servers-and-nps-servers-groups"></a>Crear grupos de usuarios de VPN, servidores VPN y servidores NPS

En este procedimiento, puede Agregar un nuevo grupo de Active Directory (AD) que contenga los usuarios a los que se les permite usar la VPN para conectarse a la red de la organización.

Este grupo tiene dos propósitos:

- Define qué usuarios pueden realizar la inscripción automática para los certificados de usuario que requiere la VPN.

- Define qué usuarios autoriza el NPS para el acceso VPN.

Mediante el uso de un grupo personalizado, si alguna vez desea revocar el acceso a la VPN de un usuario, puede quitar ese usuario del grupo.

También agregará un grupo que contenga los servidores VPN y otro grupo que contenga los servidores NPS. Estos grupos se usan para restringir las solicitudes de certificado a sus miembros.

>[!NOTE]
>Se recomienda que los servidores VPN que residen en el DMA/perímetro no estén Unidos a un dominio. Sin embargo, si prefiere que los servidores VPN estén Unidos a un dominio para una mejor capacidad de administración (directivas de grupo, agente de copia de seguridad y supervisión, sin usuarios locales para administrar, etc.), agregue un grupo de AD a la plantilla de certificado de servidor VPN.

### <a name="configure-the-vpn-users-group"></a>Configurar el grupo de usuarios de VPN

1. En un controlador de dominio, abra Active Directory usuarios y equipos.

2. Haga clic con el botón secundario en un contenedor o unidad organizativa, seleccione **nuevo**y, a continuación, seleccione **Grupo**.

3. En **nombre de grupo**, escriba **usuarios de VPN**y, luego, haga clic en **Aceptar**.

4. Haga clic con el botón derecho en **usuarios de VPN** y seleccione **propiedades**.

5. En la pestaña **miembros** del cuadro de diálogo Propiedades de los usuarios de VPN, seleccione **Agregar**.

6. En el cuadro de diálogo Seleccionar usuarios, agregue todos los usuarios que necesitan acceso a VPN y haga clic en **Aceptar**.

7. Cierre Usuarios y equipos de Active Directory.

### <a name="configure-the-vpn-servers-and-nps-servers-groups"></a>Configurar los grupos servidores VPN y servidores NPS

1. En un controlador de dominio, abra Active Directory usuarios y equipos.

2. Haga clic con el botón secundario en un contenedor o unidad organizativa, seleccione **nuevo**y, a continuación, seleccione **Grupo**.

3. En **nombre de grupo**, escriba **servidores VPN**y, luego, haga clic en **Aceptar**.

4. Haga clic con el botón secundario en **servidores VPN** y seleccione **propiedades**.

5. En la pestaña **miembros** del cuadro de diálogo Propiedades de servidores VPN, seleccione **Agregar**.

6. Seleccione **tipos de objeto**, active la casilla **equipos** y, a continuación, haga clic en **Aceptar**.

7. En **Escriba los nombres de objeto que desea seleccionar**, escriba los nombres de los servidores VPN y, luego, haga clic en **Aceptar**.

8. Seleccione **Aceptar** para cerrar el cuadro de diálogo Propiedades de servidores VPN.

9. Repita los pasos anteriores para el grupo de servidores NPS.

10. Cierre Usuarios y equipos de Active Directory.

## <a name="create-the-user-authentication-template"></a>Crear la plantilla de autenticación de usuario

En este procedimiento, configurará una plantilla de autenticación cliente-servidor personalizada. Esta plantilla es necesaria porque desea mejorar la seguridad global del certificado seleccionando niveles de compatibilidad actualizados y seleccionando el proveedor de criptografía de la plataforma Microsoft. Este último cambio le permite usar el TPM en los equipos cliente para proteger el certificado. Para obtener información general sobre el TPM, consulte [información general sobre la tecnología de módulo de plataforma segura](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview).

>[!IMPORTANT] 
>El proveedor de cifrado de plataforma de Microsoft "requiere un chip de TPM, en el caso de que se esté ejecutando una máquina virtual y se reciba el siguiente error:" no se puede encontrar un CSP válido en el equipo local "al intentar inscribir manualmente el certificado debe comprobar" almacenamiento de claves de software de Microsoft Y haga que sea segundo en orden después de "proveedor de cifrado de plataforma de Microsoft" en la pestaña criptografía de propiedades de certificado.

**Pasos**

1. En la CA, abra entidad de certificación.

2. En el panel de navegación, haga clic con el botón secundario en **plantillas de certificado** y seleccione **administrar**.

3. En la consola de plantillas de certificado, haga clic con el botón secundario en **usuario** y seleccione **duplicar plantilla**.

   >[!WARNING]
   >No seleccione **aplicar** o **Aceptar** en ningún momento anterior al paso 10.  Si selecciona estos botones antes de especificar todos los parámetros, muchas opciones se corrigen y ya no se pueden editar. Por ejemplo, en la pestaña **Criptografía** , si el _proveedor de almacenamiento criptográfico heredado_ se muestra en el campo categoría del proveedor, se deshabilitará, evitando cualquier cambio adicional. La única alternativa es eliminar la plantilla y volver a crearla.  

4. En el cuadro de diálogo Propiedades de plantilla nueva, en la pestaña **General** , complete los pasos siguientes:

   1. En **nombre para mostrar**de la plantilla, escriba **autenticación de usuario de VPN**.

   2. Desactive la casilla **publicar certificado en Active Directory** .

5. En la pestaña **seguridad** , lleve a cabo los pasos siguientes:

   1. Seleccione **Agregar**.

   2. En el cuadro de diálogo Seleccionar usuarios, equipos, cuentas de servicio o grupos, escriba **usuarios de VPN**y, luego, haga clic en **Aceptar**.

   3. En **nombres de grupos o usuarios**, seleccione **usuarios de VPN**.

   4. En **permisos para usuarios de VPN**, active las casillas **inscribir** e **inscripción automática** en la columna **permitir** .

      >[!TIP]
      >Asegúrese de mantener activada la casilla de verificación leer. En otras palabras, necesita los permisos de lectura para la inscripción. 

   5. En **nombres de grupos o usuarios**, seleccione **usuarios del dominio**y, a continuación, seleccione **quitar**.

6. En la pestaña **compatibilidad** , complete los pasos siguientes:

   1. En **entidad de certificación**, seleccione **Windows Server 2012 R2**.

   2. En el cuadro de diálogo **cambios resultantes** , haga clic en **Aceptar**.

   3. En **destinatario del certificado**, seleccione **Windows 8.1/Windows Server 2012 R2**.

   4. En el cuadro de diálogo **cambios resultantes** , haga clic en **Aceptar**.

7. En la pestaña tratamiento de la **solicitud** , desactive la casilla **permitir que la clave privada se pueda exportar** .

8. En la pestaña **Criptografía** , complete los pasos siguientes:

   1. En **categoría de proveedor**, seleccione **proveedor de almacenamiento de claves**.

   2. **Las solicitudes Select deben usar uno de los siguientes proveedores**.

   3. Active la casilla **proveedor de criptografía de plataforma de Microsoft** .

9. En la pestaña **nombre de sujeto** , si no tiene una dirección de correo electrónico en todas las cuentas de usuario, desactive las casillas **incluir nombre de correo electrónico en nombre de sujeto** y nombre de **correo electrónico** .

10. Seleccione **Aceptar** para guardar la plantilla de certificado de autenticación de usuario de VPN.

11. Cierre la consola Plantillas de certificado.

12. En el panel de navegación del complemento entidad de certificación, haga clic con el botón secundario en **plantillas de certificado**, seleccione **nueva** y, a continuación, seleccione la **plantilla de certificado que se va a emitir**.

13. Seleccione **autenticación de usuario de VPN**y haga clic en **Aceptar**.

14. Cierre el complemento entidad de certificación.

## <a name="create-the-vpn-server-authentication-template"></a>Crear la plantilla de autenticación de servidor VPN

En este procedimiento, puede configurar una nueva plantilla de autenticación de servidor para el servidor VPN. La adición de la Directiva de aplicación intermedia IKE de seguridad IP (IPsec) permite que el servidor filtre los certificados si hay más de un certificado disponible con el uso mejorado de clave de autenticación de servidor.

>[!IMPORTANT]
>Dado que los clientes VPN tienen acceso a este servidor desde la red Internet pública, los nombres de asunto y alternativos son diferentes del nombre del servidor interno. Como resultado, no puede inscribir automáticamente este certificado en servidores VPN.

**Requisitos previos**

Servidores VPN Unidos a un dominio

**Pasos**

1. En la CA, abra entidad de certificación.

2. En el panel de navegación, haga clic con el botón secundario en **plantillas de certificado** y seleccione **administrar**.

3. En la consola de plantillas de certificado, haga clic con el botón secundario en **servidor RAS e IAS** y seleccione **duplicar plantilla**.

4. En el cuadro de diálogo Propiedades de plantilla nueva, en la pestaña **General** , **en nombre para mostrar de plantilla**, escriba un nombre descriptivo para el servidor VPN, por ejemplo, autenticación de **servidor VPN** o **servidor RADIUS**.

5. En la pestaña **extensiones** , complete los pasos siguientes:

    1. Seleccione **directivas de aplicación**y, a continuación, seleccione **Editar**.

    2. En el cuadro de diálogo **Editar extensión de directivas de aplicación** , seleccione **Agregar**.

    3. En el cuadro de diálogo **Agregar Directiva de aplicación** , seleccione **seguridad IP IKE intermedio**y, después, haga clic en **Aceptar**.
   
        La adición de seguridad IP IKE intermedia al EKU ayuda en escenarios en los que existe más de un certificado de autenticación de servidor en el servidor VPN. Cuando la seguridad IP IKE intermedia está presente, IPSec solo usa el certificado con ambas opciones de EKU. Sin esto, la autenticación de IKEv2 podría producir el error 13801: las credenciales de autenticación IKE no son aceptables.

    4. Seleccione **Aceptar** para volver al cuadro **de diálogo Propiedades de plantilla nueva** .

6. En la pestaña **seguridad** , lleve a cabo los pasos siguientes:

    1. Seleccione **Agregar**.

    2. En el cuadro de diálogo **Seleccionar usuarios, equipos, cuentas de servicio o grupos** , escriba **servidores VPN**y, luego, haga clic en **Aceptar**.

    3. En **nombres de grupos o usuarios**, seleccione **servidores VPN**.

    4. En **permisos de servidores VPN**, active la casilla **inscribir** en la columna **permitir** .

    5. En **nombres de grupos o usuarios**, seleccione **servidores RAS e IAS**y, a continuación, seleccione **quitar**.

7. En la pestaña **nombre de sujeto** , complete los pasos siguientes:

    1. Seleccione **suministrar en la solicitud**.

    2. En el cuadro de diálogo Advertencia de **plantillas de certificado** , seleccione **Aceptar**.

8. Opta Si está configurando el acceso condicional para la conectividad VPN, seleccione la pestaña tratamiento de la **solicitud** y, a continuación, seleccione **permitir que la clave privada se pueda exportar**.

9. Seleccione **Aceptar** para guardar la plantilla de certificado de servidor VPN.

10. Cierre la consola Plantillas de certificado.

11. En el panel de navegación del complemento entidad de certificación, haga clic con el botón secundario en **plantillas de certificado**, haga clic en **nueva** y, a continuación, haga clic en **plantilla de certificado para emitir**.

12. Reinicie los servicios de la entidad de certificación. (*)

13. En el panel de navegación del complemento entidad de certificación, haga clic con el botón secundario en **plantillas de certificado**, seleccione **nueva** y, a continuación, seleccione la **plantilla de certificado que se va a emitir**.

14. Seleccione el nombre que eligió en el paso 4 anterior y haga clic en **Aceptar**.

15. Cierre el complemento entidad de certificación.

* **Para detener o iniciar el servicio de la entidad de certificación, ejecute el siguiente comando en CMD:**

```
Net Stop "certsvc"
Net Start "certsvc"
```

## <a name="create-the-nps-server-authentication-template"></a>Crear la plantilla de autenticación de servidor NPS

La tercera y última plantilla de certificado que se va a crear es la plantilla de autenticación de servidor NPS. La plantilla de autenticación de servidor NPS es una simple copia de la plantilla de servidor RAS e IAS protegida en el grupo de servidores NPS que creó anteriormente en esta sección.

Configurará este certificado para la inscripción automática.

**Pasos**

1. En la CA, abra entidad de certificación.

2. En el panel de navegación, haga clic con el botón secundario en **plantillas de certificado** y seleccione **administrar**.

3. En la consola de plantillas de certificado, haga clic con el botón secundario en **servidor RAS e IAS**y seleccione **duplicar plantilla**.

4. En el cuadro de diálogo Propiedades de plantilla nueva, en la pestaña **General** , en **nombre para mostrar de plantilla**, escriba autenticación de **servidor NPS**.

5. En la pestaña **seguridad** , lleve a cabo los pasos siguientes:

    1. Seleccione **Agregar**.

    2. En el cuadro de diálogo **Seleccionar usuarios, equipos, cuentas de servicio o grupos** , escriba **servidores NPS**y, a continuación, haga clic en **Aceptar**.

    3. En **nombres de grupos o usuarios**, seleccione **servidores NPS**.

    4. En **permisos para servidores NPS**, active las casillas **inscribir** e **inscripción automática** en la columna **permitir** .

    5. En **nombres de grupos o usuarios**, seleccione **servidores RAS e IAS**y, a continuación, seleccione **quitar**.

6. Seleccione **Aceptar** para guardar la plantilla de certificado de servidor NPS.

7. Cierre la consola Plantillas de certificado.

8. En el panel de navegación del complemento entidad de certificación, haga clic con el botón secundario en **plantillas de certificado**, seleccione **nueva** y, a continuación, seleccione la **plantilla de certificado que se va a emitir**.

9. Seleccione **autenticación de servidor NPS**y haga clic en **Aceptar**.

10. Cierre el complemento entidad de certificación.

## <a name="enroll-and-validate-the-user-certificate"></a>Inscripción y validación del certificado de usuario

Dado que está usando directiva de grupo para la inscripción automática de certificados de usuario, solo necesita actualizar la Directiva y Windows 10 inscribirá automáticamente la cuenta de usuario para el certificado correcto. Después, puede validar el certificado en la consola certificados.

**Pasos**

1. Inicie sesión en un equipo cliente unido a un dominio como miembro del grupo de **usuarios de VPN** .

2. Presione tecla Windows + R, escriba **gpupdate/force**y presione Entrar.

3. En el menú Inicio, escriba **Certmgr. msc**y presione Entrar.

4. En el complemento certificados, en **personal**, seleccione **certificados**. Los certificados aparecen en el panel de detalles.

5. Haga clic con el botón secundario en el certificado que tenga el nombre de usuario actual y seleccione **abrir**.

6. En la pestaña **General** , confirme que la fecha enumerada en **válido desde** es la fecha de hoy. Si no es así, es posible que haya seleccionado el certificado equivocado.

7. Seleccione **Aceptar**y cierre el complemento certificados.

## <a name="enroll-and-validate-the-server-certificates"></a>Inscribir y validar los certificados de servidor

A diferencia del certificado de usuario, debe inscribir manualmente el certificado del servidor VPN. Después de haberlo inscrito, valide mediante el mismo proceso que usó para el certificado de usuario. Al igual que el certificado de usuario, el servidor NPS inscribe automáticamente su certificado de autenticación, por lo que todo lo que necesita hacer es validarlo.

>[!NOTE]
>Es posible que tenga que reiniciar los servidores VPN y NPS para que puedan actualizar sus pertenencias a grupos para poder completar estos pasos.

### <a name="enroll-and-validate-the-vpn-server-certificate"></a>Inscripción y validación del certificado de servidor VPN

1. En el menú Inicio del servidor VPN, escriba **certlm. msc**y presione Entrar.

2. Haga clic con el botón derecho en **personal**, seleccione **todas las tareas** y luego seleccione **solicitar nuevo certificado** para iniciar el Asistente para inscripción de certificados.

3. En la página antes de comenzar, seleccione **siguiente**.

4. En la página seleccionar Directiva de inscripción de certificados, seleccione **siguiente**.

5. En la página solicitar certificados, active la casilla que hay al lado del servidor VPN para seleccionarlo.

6. En la casilla servidor VPN, seleccione **se necesita más información** para abrir el cuadro de diálogo Propiedades de certificado y realice los pasos siguientes:

    1. Seleccione la pestaña **sujeto** , seleccione **nombre común** en **nombre del sujeto**, en **tipo**.

    2. En **nombre del firmante**, en **valor**, escriba el nombre de los clientes de dominio externo usados para conectarse a la VPN, por ejemplo, VPN.contoso.com y, a continuación, seleccione **Agregar**.

    3. En **nombre alternativo**, en **tipo**, seleccione **DNS**.

    4. En **nombre alternativo**, en **valor**, escriba todos los nombres de servidor que usan los clientes para conectarse a la VPN, por ejemplo, VPN.contoso.com, VPN, 132.64.86.2.

    5. Seleccione **Agregar** después de escribir cada nombre.

    6. Cuando termine, seleccione **Aceptar** .

7. Seleccione **inscribir**.

8. Seleccione **Finalizar**.

9. En el complemento certificados, en **personal**, seleccione **certificados**.
    
    Los certificados que aparecen en la lista aparecen en el panel de detalles.

10. Haga clic con el botón secundario en el certificado que tenga el nombre del servidor VPN y seleccione **abrir**.

11. En la pestaña **General** , confirme que la fecha enumerada en **válido desde** es la fecha de hoy. Si no es así, es posible que haya seleccionado el certificado incorrecto.

12. En la pestaña **detalles** , seleccione **uso mejorado de clave**y compruebe que la **seguridad IP IKE intermedia** y la autenticación de **servidor** se muestran en la lista.

13. Seleccione **Aceptar** para cerrar el certificado.

14. Cierre el complemento Certificados.

### <a name="validate-the-nps-server-certificate"></a>Validar el certificado de servidor NPS

1. Reinicie el servidor NPS.

2. En el menú Inicio del servidor NPS, escriba **certlm. msc**y presione Entrar.

3. En el complemento certificados, en **personal**, seleccione **certificados**.

    Los certificados que aparecen en la lista aparecen en el panel de detalles.

4. Haga clic con el botón secundario en el certificado que tenga el nombre del servidor NPS y, a continuación, seleccione **abrir**.

5. En la pestaña **General** , confirme que la fecha enumerada en **válido desde** es la fecha de hoy. Si no es así, es posible que haya seleccionado el certificado incorrecto.

6. Seleccione **Aceptar** para cerrar el certificado.

7. Cierre el complemento Certificados.

## <a name="next-steps"></a>Pasos siguientes

[Paso 3. Configurar el servidor de acceso remoto para Always On VPN](vpn-deploy-ras.md): en este paso, configurará VPN de acceso remoto para permitir conexiones VPN de IKEv2, denegar conexiones desde otros protocolos VPN y asignar un grupo de direcciones IP estáticas para la emisión de direcciones IP a los clientes VPN autorizados de conexión.

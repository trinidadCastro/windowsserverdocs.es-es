---
title: Configurar la infraestructura de servidor
description: En este paso, instalar y configurar los componentes del lado servidor necesarios para admitir la VPN. Los componentes del lado servidor incluyen la configuración de PKI para distribuir los certificados usados por los usuarios, el servidor VPN y el servidor NPS.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.reviewer: deverette
ms.openlocfilehash: 21b494bea1990fb8424537205db483d977331465
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067528"
---
# Paso 2. Configurar la infraestructura de servidor

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Anterior:** paso 1. Planear la implementación de VPN siempre activada](always-on-vpn-deploy-planning.md)<br>
& #187;  [ **Siguiente:** paso 3. Configurar el servidor de acceso remoto para siempre en VPN](vpn-deploy-ras.md)


En este paso, instalar y configurar los componentes del lado servidor necesarios para admitir la VPN. Los componentes del lado servidor incluyen la configuración de PKI para distribuir los certificados usados por los usuarios, el servidor VPN y el servidor NPS.  También configurar RRAS para admitir las conexiones IKEv2 y el servidor NPS para realizar la autorización para las conexiones de VPN.

## Configurar la inscripción automática de certificados en la directiva de grupo
En este procedimiento, se configura la directiva de grupo en el controlador de dominio para que los miembros del dominio solicitan automáticamente los certificados de usuario y de equipo. Esto permite que los usuarios VPN solicitar y recuperar certificados de usuario para autentican las conexiones VPN automáticamente. De igual modo, esta directiva permite servidores NPS para solicitar el servidor de certificados de autenticación automáticamente. 

Inscribir manualmente certificados en servidores VPN.

>[!TIP]
>Para los equipos unidos no domained, consulta a continuación la [configuración de la entidad de certificación para que no son de dominio unido a los equipos](#ca-configuration-for-non-domain-joined-computers) . Dado que el servidor RRAS no está unido a un dominio, la inscripción automática no puede usarse para inscribir el certificado de puerta de enlace VPN.  Por lo tanto, usa un procedimiento de solicitud de certificado sin conexión. 


1.  En un controlador de dominio, abre Administración de directivas de grupo.

2.  En el panel de navegación, haz clic en el dominio (por ejemplo, corp.contoso.com) y haz clic en**crear un GPO en este dominio y vincularlo aquí**.

3.  En el cuadro de diálogo nuevo GPO, escriba la **Directiva de inscripción automática**y haga clic en **Aceptar**.

4.  En el panel de navegación, haz clic en la **Directiva de inscripción automática**y haz clic en **Editar**.

5.  En el Editor de administración de directivas de grupo, realiza los siguientes pasos para configurar la inscripción automática de certificados de equipo:

    1.  En el panel de navegación, haz clic en **Directivas de equipo Configuration\\Policies\\Windows seguridad Settings\\Public clave**.

    2.  En el panel de detalles, haz clic en**Servicios de cliente: inscripción automática de certificados**y haga clic en **Propiedades**.

    3.  En el cliente de servicios de certificado: cuadro de diálogo de propiedades de la inscripción automática en el **Modelo de configuración**, haz clic en **habilitado**.

    4.  Seleccione**Renovar certificados expirados, actualizar certificados pendientes y quitar certificados revocados**y**Actualizar certificados que usan plantillas de certificado**.

    5.  Haz clic en**Aceptar**.

6.  En el Editor de administración de directivas de grupo, realiza los siguientes pasos para la inscripción automática de certificados de usuario de configurar:

    1.  En el panel de navegación, haz clic en **Directivas de clave de usuario Configuration\\Policies\\Windows seguridad Settings\\Public**.

    2.  En el panel de detalles, haz clic en**Servicios de cliente: inscripción automática de certificados**y haga clic en **Propiedades**.

    3.  En el cliente de servicios de certificado: cuadro de diálogo de propiedades de la inscripción automática en el **Modelo de configuración**, haz clic en **habilitado**.

    4.  Seleccione**Renovar certificados expirados, actualizar certificados pendientes y quitar certificados revocados**y**Actualizar certificados que usan plantillas de certificado**.

    5.  Haz clic en**Aceptar**.

    6.  Cierra el Editor de administración de directivas de grupo.

7.  Administración de directivas de grupo de cerrar.

### Configuración de la entidad de certificación para los equipos de dominio no se hayan unido

Dado que el servidor RRAS no está unido a un dominio, la inscripción automática no puede usarse para inscribir el certificado de puerta de enlace VPN.  Por lo tanto, usa un procedimiento de solicitud de certificado sin conexión. 

1. En el servidor RRAS, generar un archivo llamado **VPNGateway.inf** en función de la directiva de certificado de ejemplo solicitar proporcionado en el apéndice (sección 0) y personalizar las siguientes entradas: 

   - En la sección [NewRequest], reemplaza vpn.contoso.com que se usa para el nombre del firmante con el punto de conexión de [_cliente_] VPN FQDN elegida.

   - En la sección [Extensions], reemplaza vpn.contoso.com que se usa para el nombre alternativo del sujeto con el punto de conexión [_cliente_] VPN elegido FQDN.

2. Guardar o copiar el archivo **VPNGateway.inf** a una ubicación elegida.

3. Desde un símbolo del sistema con privilegios elevados, navega hasta la carpeta que contiene el archivo **VPNGateway.inf** y el tipo:

   ```
   certreq -new VPNGateway.inf VPNGateway.req
   ```

4. Copia el archivo de salida **VPNGateway.req** recién creado en un servidor de entidad de certificación o la estación de trabajo con privilegios de acceso (PATA). 

5. Guardar o copiar el archivo **VPNGateway.req** en una ubicación seleccionada en el servidor de entidad de certificación, o la estación de trabajo con privilegios de acceso (PATA).

6. Desde un símbolo del sistema con privilegios elevados, navega hasta la carpeta que contiene el archivo VPNGateway.req creado en el paso anterior y el tipo: 

   ```
   certreq -attrib “CertificateTemplate:[Customer]VPNGateway” -submit VPNgateway.req VPNgateway.cer
   ```

7. Si se te solicite por la ventana de la lista de la entidad de certificación, selecciona la CA de empresa adecuado para satisfacer la solicitud de certificado.

8. Copia el archivo de salida **VPNGateway.cer** recién creado en el servidor RRAS. 

9. Guardar o copiar el archivo **VPNGateway.cer** a una ubicación en el servidor RRAS elegida.

10. Desde un símbolo del sistema con privilegios elevados, navega hasta la carpeta que contiene el archivo VPNGateway.cer creado en el paso anterior y el tipo:
   
   ```
   certreq -accept VPNGateway.cer
   ```

11. Ejecuta el complemento MMC de certificados como se describe [aquí](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in) seleccionando la opción de **cuenta de equipo** .

12. Asegúrate de que existe un certificado válido para el servidor RRAS con las siguientes propiedades:

   - **Destinada a fines:** Autenticación de servidor, seguridad IP IKE intermedia 

   - **Plantilla de certificado:** [El_cliente_] Servidor VPN

#### Ejemplo: Script VPNGateway.inf

Aquí puedes ver un script de ejemplo de una directiva de solicitud de certificado usado para solicitar un certificado de puerta de enlace VPN mediante un proceso de fuera de banda.

>[!TIP]
>Puedes encontrar una copia de la secuencia de comandos VPNGateway.inf en el Kit IP ofrecer VPN en la carpeta de directivas de solicitud de certificado. Solo se actualiza el 'Asunto' y '\_continue\_' con valores específicos de cliente.

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

## Crear los usuarios de VPN, servidores VPN y grupos de servidores NPS

En este procedimiento, puedes agregar un nuevo grupo de Active Directory (AD) que contiene los usuarios puedos usar la conexión VPN para conectarse a la red de la organización.

Este grupo tiene dos funciones:

-   Define qué usuarios pueden inscribir automáticamente para los certificados de usuario que requiere que la VPN.

-   Define qué usuarios NPS autoriza acceso a la VPN.

Mediante el uso de un grupo personalizado, si quieres revocar el acceso VPN de un usuario, puedes quitar ese usuario del grupo.

También puedes agregar un grupo que contiene servidores VPN y otro grupo que contiene servidores NPS. Usa estos grupos para restringir las solicitudes de certificados a sus miembros.

>[!NOTE] 
>Te recomendamos VPN servidores que se encuentran en el perímetro de DMA no estar unidos al dominio. Sin embargo, si prefieres que los servidores VPN unido al dominio para facilitar la tarea (directivas de grupo, el agente de copia de seguridad y supervisión, ningún usuario local para administrar etc.), a continuación, agregar un grupo de AD para la plantilla de certificado de servidor VPN.

### Configurar el grupo de usuarios de VPN

1.  En un controlador de dominio, abre equipos y usuarios de Active Directory.

2.  Haz clic en un contenedor o la unidad organizativa, haga clic en **nuevo** y haz clic en el **grupo**.

3.  En **nombre de grupo**, escribe **Los usuarios de VPN**y haz clic en **Aceptar**.

4.  Haz clic en **Los usuarios de VPN**y haga clic en **Propiedades**.

5.  En la pestaña **miembros** del cuadro de diálogo de propiedades de los usuarios de VPN, haz clic en **Agregar**.

6.  En el cuadro de diálogo Seleccionar usuarios, agregar todos los usuarios que necesiten VPN acceder y haz clic en **Aceptar**.

7.  Cierra los equipos y usuarios de Active Directory.

### Configurar los grupos de servidores VPN y servidores NPS

1.  En un controlador de dominio, abre equipos y usuarios de Active Directory.

2.  Haz clic en un contenedor o la unidad organizativa, haga clic en **nuevo** y haz clic en el **grupo**.

3.  En **nombre de grupo**, escribe **Los servidores VPN**y haz clic en **Aceptar**.

4.  Haz clic en **Los servidores VPN**y haga clic en **Propiedades**.

5.  En la pestaña **miembros** del cuadro de diálogo de propiedades de servidores VPN, haz clic en **Agregar**.

6.  Haz clic en **Los tipos de objeto**, selecciona la casilla de verificación de **equipos** y haz clic en **Aceptar**.

7.  En **Escriba los nombres de objeto que desea seleccionar**, escriba los nombres de los servidores VPN y haz clic en **Aceptar**.

8.  Haz clic en **Aceptar** para cerrar el cuadro de diálogo de propiedades de servidores VPN.

9.  Repite los pasos anteriores para el grupo de servidores NPS.

10. Cierra los equipos y usuarios de Active Directory.

## Crear la plantilla de autenticación de usuario

En este procedimiento, se configura una plantilla de autenticación de servidor de cliente personalizada. Esta plantilla es necesaria porque para mejorar la seguridad general del certificado seleccionando niveles de compatibilidad actualizada y eligiendo el proveedor criptográfico de plataforma de Microsoft. Este último cambio te permite usar el TPM en los equipos cliente para proteger el certificado. Para obtener información general del TPM, vea la [Descripción de la tecnología del módulo de plataforma de confianza](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview).

**Procedimiento:**

1.  En la entidad de certificación, abre la entidad de certificación.

2.  En el panel de navegación, haz clic en**Las plantillas de certificado**y haga clic en**Administrar**.

3.  En la consola de plantillas de certificado, haz clic en el**usuario**y haz clic en la**Plantilla duplicada**.

    >[!WARNING]
    >No hagas clic en **Aplicar** o **Aceptar** en cualquier momento antes de paso 10.  Si haces clic en estos botones antes de entrar en todos los parámetros, muchas opciones quedan fijo y ya no modificable. Por ejemplo, en la pestaña de **criptografía** , si el _Proveedor de almacenamiento criptográfico heredado_ se muestra en el campo de categoría del proveedor, se convierte en disabled, evitando que cualquier otro cambio. La única alternativa es eliminar la plantilla y volver a crearla.  

4.  En el cuadro de diálogo de propiedades de plantilla nueva, en**General**pestaña, realiza los siguientes pasos:

    1.  En el **nombre para mostrar plantilla**, escriba la **Autenticación de usuario de VPN**.

    2.  Desactiva la casilla de verificación **Publicar certificado en Active Directory** .

5.  En la**seguridad**pestaña, realiza los siguientes pasos:

    1.  Haz clic en **Agregar**.

    2.  En el seleccionar usuarios, equipos, cuentas de servicio o cuadro de diálogo de grupos, escribe **Los usuarios de VPN**y haz clic en **Aceptar**.

    3.  En los **nombres de usuario o grupo**, haz clic en **Los usuarios de VPN**.

    4.  En los **permisos para usuarios de VPN**, selecciona las casillas de verificación de **inscripción** e **inscripción automática** en la columna **Permitir** .

       >[!TIP]
       >Asegúrese de que mantenga activada la casilla de verificación de lectura. En otras palabras, necesitas los permisos de lectura para la inscripción. 

    5.  En **los nombres de usuario o grupo**, haz clic en **Los usuarios del dominio**y haz clic en **Eliminar**.

6.  En la pestaña de la **compatibilidad** , realiza los siguientes pasos:

    1.  En la **Entidad de certificación**, haz clic en **Server2012R2 de Windows**.

    2.  En el cuadro de diálogo **resultante cambios** , haz clic en **Aceptar**.

    3.  En el **destinatario del certificado**, haz clic en **Server2012R2 Windows8.1/Windows**.

    4.  En el cuadro de diálogo **resultante cambios** , haz clic en **Aceptar**.

7.  En la ficha **Tratamiento de la solicitud** , desactiva la casilla de verificación de **Permitir que la clave privada se pueda exportar** .

8.  En la pestaña de **criptografía** , realiza los siguientes pasos:

    1.  En la **Categoría del proveedor**, haz clic en **El proveedor de almacenamiento de claves**.

    2.  Haz clic en **las solicitudes deben usar uno de los siguientes proveedores**.

    3.  Selecciona la casilla de verificación de **Proveedor a criptográfico de plataforma de Microsoft** .

9.  En la pestaña de **Nombre de sujeto** , si no tienes una dirección de correo electrónico que aparece en todas las cuentas de usuario, desactiva las casillas de verificación de **incluir el nombre de correo electrónico en nombre de sujeto** y el **nombre de correo electrónico** .

10. Haz clic en**Aceptar** para guardar la plantilla de certificado de autenticación de usuario de VPN.

11. Consola de plantillas de certificado cerrar.

12. En el panel de navegación de theCertification Authoritysnap-, haz clic en**Plantillas de certificado**, haz clic en **nuevo** y, a continuación, haz clic en**La plantilla de certificado para emitir**.

13. Haz clic en**La autenticación de usuario de VPN**y haz clic en**Aceptar**.

14. Cierra el complemento theCertification entidad.

## Crear la plantilla de autenticación de servidor VPN

En este procedimiento, puedes configurar una nueva plantilla de autenticación de servidor para el servidor VPN. Agregar que la directiva de aplicación de seguridad IP (IPsec) IKE Intermediate permite que el servidor de certificados de filtro si hay más de un certificado con la autenticación de servidor extiende el uso de la clave.

>[!IMPORTANT] 
>Dado que los clientes VPN acceder a este servidor desde Internet pública, el asunto y los nombres alternativos son distintos del nombre de servidor interno. Como resultado, no puedes inscribir automáticamente este certificado en servidores VPN.

**Requisitos previos:**<p>
Servidores VPN unido al dominio

**Procedimiento:** 

1.  En la entidad de certificación, abre la entidad de certificación.

2.  En el panel de navegación, haz clic en **Las plantillas de certificado**y haga clic en**Administrar**.

3.  En la consola de plantillas de certificado, haz clic en el**servidor RAS e IAS**y haga clic en la**Plantilla duplicada**.

4.  En el cuadro de diálogo de propiedades de plantilla nueva, en**General**ficha, en el **nombre para mostrar plantilla**, escribe un nombre descriptivo para el servidor VPN, por ejemplo, **Autenticación de servidor VPN** o **Servidor RADIUS**.

5.  En las**extensiones**pestaña, realiza los siguientes pasos:

    1.  Haz clic en**Las directivas de aplicación**y haz clic en**Editar**.

    2.  En el cuadro de diálogo **Editar extensión de directivas de aplicación** , haz clic en**Agregar**.

    3.  En el cuadro de diálogo **Agregar directivas de aplicación** , haz clic en **seguridad IP IKE intermedia**y haz clic en **Aceptar**.<p>Agregar IP seguridad IKE intermedio para el EKU ayuda en escenarios donde existe más de un certificado de autenticación de servidor en el servidor VPN. Cuando está presente seguridad IP IKE intermedia, IPSec solo usa el certificado con ambas opciones EKU. Sin esta, podría producirse un error de autenticación IKEv2 con Error 13801: las credenciales de autenticación de IKE son inaceptables.

    4.  Haz clic en **Aceptar** para volver a las**Propiedades de la nueva plantilla**cuadro de diálogo.

6.  En la**seguridad**pestaña, realiza los siguientes pasos:

    1.  Haz clic en **Agregar**.

    2.  En el cuadro de diálogo **Seleccionar usuarios, equipos, cuentas de servicio o grupos** , escriba **Servidores VPN**y haz clic en **Aceptar**.

    3.  En los **nombres de usuario o grupo**, haz clic en **Los servidores VPN**.

    4.  En los **permisos de servidores VPN**, selecciona la casilla de verificación **inscribirse** en la columna **Permitir** .

    5.  En los **nombres de usuario o grupo**, haz clic en **servidores RAS e IAS**y haz clic en **Eliminar**.

7.  En la pestaña de **Nombre de sujeto** , realiza los siguientes pasos:

    1.  Haz clic en **proporcionado por el solicitante**.

    2.  En el cuadro de diálogo de advertencia de **Plantillas de certificado** , haz clic en **Aceptar**.

8.  (Opcional) *Si va a configurar el acceso condicional para la conectividad de VPN*, haz clic en la pestaña de **Tratamiento de la solicitud** y haga clic en **Permitir que la clave privada se pueda exportar** para seleccionarlo.

9.  Haz clic en**Aceptar** para guardar la plantilla de certificado de servidor VPN.

10. Consola de plantillas de certificado cerrar.

11. En el panel de navegación de la certificación en Authoritysnap, haz clic en**Las plantillas de certificado**, haz clic en **nuevo** y, a continuación, haz clic en**La plantilla de certificado para emitir**.

12. Reinicie los servicios de la entidad de certificación.

13. Selecciona el nombre que has elegido en el paso 4 anterior y haz clic en**Aceptar**.

14. Cierra el complemento theCertification entidad.

## Crear la plantilla de autenticación de servidor NPS

La plantilla de certificado terceros y el último para crear es la plantilla de autenticación de servidor NPS. La plantilla de autenticación de servidor NPS es una copia simple de la plantilla de servidor RAS e IAS protegida en el grupo de servidor NPS que creaste anteriormente en esta sección. 

Configurará este certificado para la inscripción automática.

**Procedimiento:**

1.  En la entidad de certificación, abre la entidad de certificación.

2.  En el panel de navegación, haz clic en **Las plantillas de certificado**y haga clic en**Administrar**.

3.  En la consola de plantillas de certificado, haz clic en el**servidor RAS e IAS**y selecciona la **Plantilla duplicada**.

4.  En el cuadro de diálogo de propiedades de plantilla nueva, en**General**pestaña, en el **nombre para mostrar de plantilla**, tipo de **Autenticación de servidor NPS**.

5.  En la**seguridad**pestaña, realiza los siguientes pasos:

    1.  Haz clic en **Agregar**.

    2.  En el cuadro de diálogo **Seleccionar usuarios, equipos, cuentas de servicio o grupos** , escriba **Servidores NPS**y haz clic en **Aceptar**.

    3.  En los **nombres de usuario o grupo**, haz clic en **Servidores NPS**.

    4.  En los **permisos para servidores NPS**, selecciona las casillas de verificación de **inscripción** e **inscripción automática** en la columna **Permitir** .

    5.  En los **nombres de usuario o grupo**, haz clic en **servidores RAS e IAS**y haz clic en **Eliminar**.

6.  Haz clic en**Aceptar** para guardar la plantilla de certificado de servidor NPS.

7.  Consola de plantillas de certificado cerrar.

8.  En el panel de navegación de la certificación en Authoritysnap, haz clic en**Las plantillas de certificado**, haz clic en **nuevo** y, a continuación, haz clic en**La plantilla de certificado para emitir**.

9.  Haz clic en**La autenticación de servidor NPS**y haz clic en**Aceptar**.

10. Cierra el complemento theCertification entidad.

## Inscribir y validar el certificado de usuario

Dado que estás usando la directiva de grupo para inscribir automáticamente los certificados de usuario, sólo necesita actualizar la directiva y Windows 10 se inscribirá automáticamente la cuenta de usuario para el certificado correcto. A continuación, se puede validar el certificado en la consola de certificados.

**Procedimiento:**

1.  Inicia sesión como un miembro del grupo de **Usuarios de VPN** en un equipo cliente unido al dominio.

2.  Presiona la tecla Windows + R, **escriba/Force gpupdate**y presiona ENTRAR.

3.  En el menú Inicio, escriba **certmgr.msc**y presiona ENTRAR.

4.  En el complemento de certificados **Personal**, haga clic en **certificados**. Los certificados aparecen en el panel de detalles.

5.  Haz clic en el certificado que tiene el nombre de usuario de dominio actual y, a continuación, haga clic en **Abrir**.

6.  En la pestaña **General** , confirma que la fecha que aparece bajo **válido desde** es la fecha actual. Si no es así, es posible que hayas seleccionado el certificado incorrecto.

7.  Haz clic en **Aceptar**y cierra el complemento de certificados.

## Inscribir y validar los certificados de servidor

A diferencia de los certificados de usuario, deben inscribir manualmente el certificado del servidor VPN. Después de haber inscrito, validar mediante el mismo proceso que usaste para el certificado de usuario. Al igual que el certificado de usuario, el servidor NPS inscribe automáticamente su certificado de autenticación, por lo que todo lo que necesitas hacer es validar.

>[!NOTE] 
>Debes reiniciar los servidores VPN y NPS para que puedan actualizar su pertenencia a grupos para poder completar estos pasos.

### Inscribir y validar el certificado del servidor VPN

1.  En el menú de inicio del servidor VPN, escriba **certlm.msc**y presiona ENTRAR.

2.  Haz clic en **Personal**, haz clic en **Todas las tareas** y, a continuación, haz clic en **Solicitar un nuevo certificado** para iniciar al Asistente para la inscripción de certificados.

3.  En la página Antes de comenzar, haz clic en **Siguiente**.

4.  En la página Seleccionar directiva de inscripción de certificado, haz clic en **siguiente**.

5.  En la página solicitar certificados, haz clic en la casilla de verificación junto al servidor VPN para seleccionarlo.

6.  En la casilla de verificación del servidor VPN, haz clic en **que se necesita más información** para abrir el cuadro de diálogo de propiedades de certificado y realiza los siguientes pasos:

    1.  Haga clic en la pestaña de **asunto** , **Nombre común** en **nombre de sujeto**, en **tipo**.

    2.  En **nombre de sujeto**, en un **valor**, escribe el nombre de los clientes de dominio externo que se usan para conectarse a la VPN, por ejemplo, vpn.contoso.com y haz clic en **Agregar**.

    3.  **Nombre alternativo**, en **tipo**, haz clic en **DNS**.

    4.  **Nombre alternativo**, en un **valor**, escribe todos el servidor de los clientes de nombres que se usan para conectarse a la VPN, por ejemplo, vpn.contoso.com, vpn, 132.64.86.2.

    5.  Después de escribir cada nombre, haz clic en **Agregar** .

    6.  Al finalizar, haz clic en **Aceptar**.

7.  Haz clic en **Inscribir.**

8.  Haz clic en **Finalizar**.

9.  En el complemento de certificados **Personal**, haga clic en **certificados**.<p>Los certificados que se enumeran aparecen en el panel de detalles.

10. Haz clic en el certificado que tiene el nombre del servidor VPN y, a continuación, haga clic en **Abrir**.

11. En la pestaña **General** , confirma que la fecha que aparece bajo **válido desde** es la fecha actual. Si no es así, es posible que hayas seleccionado el certificado correcto.

12. En la pestaña **Detalles** , haz clic en **Uso mejorado de claves**y comprobar que la **Autenticación de servidor** y **seguridad IP IKE intermedia** se muestran en la lista.

13. Haz clic en **Aceptar** para cerrar el certificado.

14. Cierra el complemento de certificados.

### Validar el certificado de servidor NPS

1.  Reinicia el servidor NPS.

2.  En el menú de inicio del servidor NPS, escriba **certlm.msc**y presiona ENTRAR.

3.  En el complemento de certificados **Personal**, haga clic en **certificados**.<p>Los certificados que se enumeran aparecen en el panel de detalles.

4.  Haz clic en el certificado que tiene el nombre del servidor NPS y, a continuación, haga clic en **Abrir**.

5.  En la pestaña **General** , confirma que la fecha que aparece bajo **válido desde** es la fecha actual. Si no es así, es posible que hayas seleccionado el certificado correcto.

6.  Haz clic en **Aceptar** para cerrar el certificado.

7.  Cierra el complemento de certificados.

## Paso siguiente
Paso 3 de [. Configurar el servidor de acceso remoto para VPN siempre activada](vpn-deploy-ras.md): en este paso, configuren VPN de acceso remoto para permitir conexiones VPN IKEv2, denegar conexiones desde otros protocolos VPN y asignar un conjunto de direcciones IP estáticas para su emisión de direcciones IP a la conexión clientes VPN autorizados.







---

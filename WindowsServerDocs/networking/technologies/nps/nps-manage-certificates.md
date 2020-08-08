---
title: Administrar los certificados que se usan con NPS
description: En este tema se proporciona información sobre el uso de certificados de servidor con el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 204a4ef4-9d78-4a62-9940-43cc0e1c39d0
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 0641b78a15a8d8464dd1993b62439f495391f8b1
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962039"
---
# <a name="manage-certificates-used-with-nps"></a>Administrar los certificados que se usan con NPS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Si implementa un método de autenticación basada en certificados, como protocolo de autenticación extensible \- seguridad \( EAP TLS, protocolo de \- \) autenticación extensible protegido seguridad de la capa de \- transporte \( PEAP \- TLS \) y PEAP \- Protocolo de autenticación por desafío mutuo de Microsoft versión 2 \( MS \- CHAP V2 \) , debe inscribir un certificado de servidor en todos los NPSs. El certificado de servidor debe:

- Cumplir los requisitos mínimos de certificados de servidor, tal y como se describe en [configuración de plantillas de certificado para los requisitos de PEAP y EAP](nps-manage-cert-requirements.md)

- Debe ser emitido por una CA de entidad de certificación \( \) que sea de confianza para los equipos cliente. Una CA es de confianza cuando su certificado existe en el almacén de certificados de entidades de certificación raíz de confianza para el usuario actual y el equipo local.

Las siguientes instrucciones ayudan en la administración de certificados NPS en implementaciones en las que la entidad de certificación raíz de confianza es una entidad de certificación de terceros, como VeriSign, o es una entidad de certificación que ha implementado para la PKI de la infraestructura de clave pública \( \) mediante Active Directory servicios de Certificate Server \( ad CS \) .

## <a name="change-the-cached-tls-handle-expiry"></a>Cambiar la expiración del identificador de TLS en caché

Durante los procesos de autenticación inicial de EAP \- TLS, PEAP \- TLS y PEAP \- MS \- CHAP V2, NPS almacena en caché una parte de las propiedades de conexión TLS del cliente que se conecta. El cliente también almacena en caché una parte de las propiedades de conexión TLS del NPS.

Cada colección individual de estas propiedades de conexión TLS se denomina identificador de TLS.

Los equipos cliente pueden almacenar en caché los identificadores TLS para varios autenticadores, mientras que NPSs puede almacenar en caché los identificadores TLS de muchos equipos cliente.

Los identificadores TLS almacenados en caché en el cliente y el servidor permiten que el proceso de reautenticación se produzca con más rapidez. Por ejemplo, cuando un equipo inalámbrico vuelve a autenticarse con un NPS, el NPS puede examinar el identificador de TLS para el cliente inalámbrico y puede determinar rápidamente que la conexión de cliente es una reconexión. NPS autoriza la conexión sin realizar la autenticación completa.

En consecuencia, el cliente examina el identificador de TLS para NPS, determina que es una reconexión y no necesita realizar la autenticación del servidor.

En los equipos que ejecutan Windows 10 y Windows Server 2016, la expiración del identificador de TLS predeterminado es de 10 horas.

En algunas circunstancias, puede que desee aumentar o disminuir el tiempo de expiración del identificador de TLS.

Por ejemplo, puede que desee reducir el tiempo de expiración del identificador de TLS en las circunstancias en las que un administrador revoca el certificado de un usuario y el certificado ha expirado. En este escenario, el usuario todavía puede conectarse a la red si un NPS tiene un identificador de TLS almacenado en caché que no ha expirado. Reducir la expiración del identificador de TLS puede ayudar a evitar que estos usuarios con certificados revocados vuelvan a conectarse.

>[!NOTE]
>La mejor solución para este escenario es deshabilitar la cuenta de usuario en Active Directory o quitar la cuenta de usuario del grupo de Active Directory al que se concede permiso para conectarse a la red en la Directiva de red. La propagación de estos cambios a todos los controladores de dominio también puede retrasarse, sin embargo, debido a la latencia de replicación.

## <a name="configure-the-tls-handle-expiry-time-on-client-computers"></a>Configurar la hora de expiración del identificador de TLS en los equipos cliente

Puede usar este procedimiento para cambiar la cantidad de tiempo que los equipos cliente almacenan en caché el identificador de TLS de un NPS. Después de autenticar correctamente un NPS, los equipos cliente almacenan en caché las propiedades de conexión TLS del NPS como un identificador de TLS. El identificador de TLS tiene una duración predeterminada de 10 horas \( 36 millones milisegundos \) . Puede aumentar o disminuir la hora de expiración del identificador de TLS mediante el procedimiento siguiente.

La pertenencia al grupo **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

>[!IMPORTANT]
>Este procedimiento se debe realizar en un NPS, no en un equipo cliente.

### <a name="to-configure-the-tls-handle-expiry-time-on-client-computers"></a>Para configurar la hora de expiración del identificador de TLS en los equipos cliente

1. En un NPS, abra el editor del registro.

2. Vaya a la clave del registro **HKEY \_ local \_ MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. En el menú **edición** , haga clic en **nuevo**y, a continuación, haga clic en **clave**.

4. Escriba **ClientCacheTime**y, a continuación, presione Entrar.

5. Haga clic con el botón secundario en **ClientCacheTime**, haga clic en **nuevo**y, a continuación, haga clic en **valor DWORD (32 bits)**.

6. Escriba la cantidad de tiempo, en milisegundos, que desea que los equipos cliente almacenen en caché el identificador de TLS de un NPS después del primer intento de autenticación correcto del NPS.

## <a name="configure-the-tls-handle-expiry-time-on-npss"></a>Configurar la hora de expiración del identificador de TLS en NPSs

Utilice este procedimiento para cambiar la cantidad de tiempo que NPSs almacenar en caché el identificador de TLS de los equipos cliente. Después de autenticar correctamente un cliente de acceso, NPSs almacena en caché las propiedades de conexión TLS del equipo cliente como un identificador de TLS. El identificador de TLS tiene una duración predeterminada de 10 horas \( 36 millones milisegundos \) . Puede aumentar o disminuir la hora de expiración del identificador de TLS mediante el procedimiento siguiente.

La pertenencia al grupo **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

>[!IMPORTANT]
>Este procedimiento se debe realizar en un NPS, no en un equipo cliente.

### <a name="to-configure-the-tls-handle-expiry-time-on-npss"></a>Para configurar la hora de expiración del identificador de TLS en NPSs

1. En un NPS, abra el editor del registro.

2. Vaya a la clave del registro **HKEY \_ local \_ MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. En el menú **edición** , haga clic en **nuevo**y, a continuación, haga clic en **clave**.

4. Escriba **ServerCacheTime**y, a continuación, presione Entrar.

5. Haga clic con el botón secundario en **ServerCacheTime**, haga clic en **nuevo**y, a continuación, haga clic en **valor DWORD (32 bits)**.

6. Escriba la cantidad de tiempo, en milisegundos, que desea que NPSs almacene en caché el identificador de TLS de un equipo cliente después del primer intento de autenticación correcta del cliente.

## <a name="obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>Obtener el hash SHA-1 de un certificado de CA raíz de confianza

Use este procedimiento para obtener el hash del algoritmo hash seguro (SHA-1) de una entidad de certificación (CA) raíz de confianza de un certificado que está instalado en el equipo local. En algunas circunstancias, como al implementar directiva de grupo, es necesario designar un certificado mediante el hash SHA-1 del certificado.

Al usar directiva de grupo, puede designar uno o varios certificados de CA raíz de confianza que los clientes deben usar para autenticar el NPS durante el proceso de autenticación mutua con EAP o PEAP. Para designar un certificado de CA raíz de confianza que los clientes deben usar para validar el certificado de servidor, puede especificar el hash SHA-1 del certificado.

En este procedimiento se muestra cómo obtener el hash SHA-1 de un certificado de CA raíz de confianza mediante el complemento Microsoft Management Console (MMC) de certificados.

Para completar este procedimiento, debe ser miembro del grupo **usuarios** en el equipo local.

### <a name="to-obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>Para obtener el hash SHA-1 de un certificado de CA raíz de confianza

1. En el cuadro de diálogo Ejecutar o Windows PowerShell, escriba **MMC**y, a continuación, presione Entrar. Se abre MMC de Microsoft Management Console \( \) . En MMC, haga clic en **archivo**y, a continuación, haga clic en **Agregar o quitar Snap\in**. Se abre el cuadro de diálogo **Agregar o quitar complementos**.

2. En **Agregar o quitar complementos**, en **Complementos disponibles**, haga doble clic en **Certificados**. Se abre el Asistente para complementos de certificados. Haga clic en **Cuenta de equipo** y, a continuación, en **Siguiente**.

3. En **seleccionar equipo**, asegúrese de que está seleccionado **equipo local (el equipo en el que se está ejecutando esta consola)** , haga clic en **Finalizar**y, a continuación, haga clic en **Aceptar**.

4. En el panel izquierdo, haga doble clic en **certificados (equipo local)** y, a continuación, haga doble clic en la carpeta **entidades de certificación raíz de confianza** .

5. La carpeta **certificados** es una subcarpeta de la carpeta **entidades de certificación raíz de confianza** . Haga clic en la carpeta **Certificados**.

6. En el panel de detalles, busque el certificado de la CA raíz de confianza. Haga doble clic en el certificado. Se abre el cuadro de diálogo **Certificado**.

7. En el cuadro de diálogo **Certificado**, haga clic en la pestaña **Detalles**.

8. En la lista de campos, desplácese a y seleccione **huella digital**.

9. En el panel inferior, se muestra la cadena hexadecimal que es el hash SHA-1 de su certificado. Seleccione el hash SHA-1 y, a continuación, presione el método abreviado de teclado de Windows para el comando Copiar \( Ctrl \+ C \) para copiar el hash en el portapapeles de Windows.

10. Abra la ubicación en la que desea pegar el hash SHA-1, busque correctamente el cursor y, a continuación, presione el método abreviado de teclado de Windows para el comando pegar \( Ctrl \+ V \) .

Para obtener más información acerca de los certificados y NPS, consulte [configuración de plantillas de certificado para los requisitos de PEAP y EAP](nps-manage-cert-requirements.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).

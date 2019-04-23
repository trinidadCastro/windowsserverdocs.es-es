---
title: Administrar los certificados que se usan con NPS
description: En este tema se proporciona información sobre el uso de certificados de servidor con el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 204a4ef4-9d78-4a62-9940-43cc0e1c39d0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 73f3d6a1e9dc6ae1520b1d685b6b05b5f3aed601
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864236"
---
# <a name="manage-certificates-used-with-nps"></a>Administrar los certificados que se usan con NPS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Si implementa un método de autenticación basada en certificados, como protocolo de autenticación Extensible\-Transport Layer Security \(EAP\-TLS\), protegido Protocolo de autenticación Extensible\-Transport Layer Security \(PEAP\-TLS\)y PEAP\-protocolo de autenticación de Microsoft desafío mutuo versión 2 \(MS\-CHAP v2\), debe inscribir un certificado de servidor a todos sus NPSs. El certificado de servidor debe:

- Cumplir los requisitos de certificado de servidor mínima como se describe en [configurar plantillas de certificados para PEAP y EAP requisitos](nps-manage-cert-requirements.md)

- Ser emitido por una entidad de certificación \(CA\) de confianza de los equipos cliente. Es una entidad de certificación de confianza cuando su certificado existe en el almacén de certificados de entidades de certificación raíz de confianza para el usuario actual y el equipo local.

Las instrucciones siguientes ayudarán a administrar certificados NPS en las implementaciones donde la entidad de certificación raíz de confianza es una entidad de certificación de terceros, como Verisign, o es una CA que haya implementado para la infraestructura de clave pública \(PKI\) mediante el uso de activo Servicios de certificados \(AD CS\).

## <a name="change-the-cached-tls-handle-expiry"></a>Cambiar la caducidad del identificador de TLS en caché

Durante los procesos de autenticación inicial para EAP\-TLS, PEAP\-TLS y PEAP\-MS\-CHAP v2, NPS almacena en caché una parte de las propiedades de conexión TLS de conexión del cliente. El cliente también almacena en caché una parte de las propiedades de la conexión TLS de NPS.

Cada colección individual de estas propiedades de conexión de TLS se denomina un identificador de TLS.

Los equipos cliente pueden almacenar en caché los identificadores TLS para varios autenticadores mientras NPSs puede almacenar en caché los identificadores TLS de muchos equipos cliente.

Los identificadores TLS en caché en el cliente y servidor permitir que el proceso de reautenticación para que se produzca más rápidamente. Por ejemplo, cuando vuelve a autenticar un equipo inalámbrico con NPS, NPS puede examinar el identificador de TLS para el cliente inalámbrico y puede determinar rápidamente que la conexión de cliente es una reconexión. NPS autoriza la conexión sin necesidad de realizar la autenticación completa.

En consecuencia, el cliente examina el identificador de TLS para NPS, determina que es una reconexión y no es necesario realizar la autenticación de servidor.

En los equipos que ejecutan Windows 10 y Windows Server 2016, la expiración de identificador TLS de forma predeterminada es de 10 horas.

En algunas circunstancias, es posible que desee aumentar o disminuir el tiempo de expiración del identificador TLS.

Por ejemplo, es posible que desee reducir el tiempo de expiración del identificador TLS en circunstancias donde se revoca el certificado de un usuario por un administrador y el certificado ha expirado. En este escenario, el usuario todavía puede conectarse a la red si NPS tiene un controlador en caché de TLS que no haya expirado. Lo que reduce la TLS identificador expiración puede ayudar a evitar estos usuarios con certificados revocados pueda volver a conectar.

>[!NOTE]
>La mejor solución para este escenario es para deshabilitar la cuenta de usuario en Active Directory, o para quitar la cuenta de usuario del grupo de Active Directory que se concede permiso para conectarse a la red en la directiva de red. La propagación de estos cambios a todos los controladores de dominio también se retrasa, sin embargo, debido a la latencia de replicación. 

## <a name="configure-the-tls-handle-expiry-time-on-client-computers"></a>Configurar la hora de expiración del identificador TLS en los equipos cliente

Puede usar este procedimiento para cambiar la cantidad de tiempo que los equipos cliente almacenan en caché el identificador de TLS de NPS. Después de autenticar correctamente un NPS, los equipos cliente almacenar en caché las propiedades de conexión TLS de NPS como un identificador de TLS. El identificador de TLS tiene una duración predeterminada de 10 horas \(36,000,000 milisegundos\). Puede aumentar o disminuir el tiempo de expiración del identificador TLS mediante el procedimiento siguiente.

Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

>[!IMPORTANT]
>Este procedimiento debe realizarse en NPS, no en un equipo cliente.

### <a name="to-configure-the-tls-handle-expiry-time-on-client-computers"></a>Para configurar TLS controlar la hora de expiración en los equipos cliente

1. En NPS, abra el Editor del registro.

2. Vaya a la clave del registro **HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. En el **editar** menú, haga clic en **New**y, a continuación, haga clic en **clave**.

4. Tipo **ClientCacheTime**, y, a continuación, presione ENTRAR.

5. Haga clic en **ClientCacheTime**, haga clic en **New**y, a continuación, haga clic en **valor DWORD (32 bits)**.

6. Escriba la cantidad de tiempo, en milisegundos, que desea que los equipos cliente para almacenar en caché el identificador de TLS de NPS después de intentar la primera autenticación correcta en el NPS.

## <a name="configure-the-tls-handle-expiry-time-on-npss"></a>Configurar la hora de expiración del identificador TLS en NPSs

Utilice este procedimiento para cambiar la cantidad de tiempo que NPSs almacena en caché el identificador de TLS de los equipos cliente. Después de autenticar correctamente un cliente de acceso, NPSs almacenar en caché las propiedades de conexión TLS del equipo cliente como un identificador de TLS. El identificador de TLS tiene una duración predeterminada de 10 horas \(36,000,000 milisegundos\). Puede aumentar o disminuir el tiempo de expiración del identificador TLS mediante el procedimiento siguiente.

Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

>[!IMPORTANT]
>Este procedimiento debe realizarse en NPS, no en un equipo cliente.

### <a name="to-configure-the-tls-handle-expiry-time-on-npss"></a>Para configurar TLS controlar la hora de expiración en NPSs

1. En NPS, abra el Editor del registro.

2. Vaya a la clave del registro **HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. En el **editar** menú, haga clic en **New**y, a continuación, haga clic en **clave**.

4. Tipo **ServerCacheTime**, y, a continuación, presione ENTRAR.

5. Haga clic en **ServerCacheTime**, haga clic en **New**y, a continuación, haga clic en **valor DWORD (32 bits)**.

6. Escriba la cantidad de tiempo, en milisegundos, que desea NPSs para almacenar en caché el identificador de TLS de un equipo cliente después de la primera autenticación correcta intente por el cliente.

## <a name="obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>Obtener el Hash SHA-1 de un certificado de CA raíz de confianza

Utilice este procedimiento para obtener el algoritmo Hash seguro (SHA-1) de hash de una entidad de certificación raíz de confianza (CA) de un certificado que está instalado en el equipo local. En algunas circunstancias, como al implementar la directiva de grupo, es necesario designar un certificado mediante el hash SHA-1 del certificado.

Cuando se usa la directiva de grupo, puede designar uno o varios certificados de CA raíz de confianza que los clientes deben usar con el fin de autenticar el NPS durante el proceso de autenticación mutua con EAP o PEAP. Para designar un certificado de CA raíz de confianza que los clientes deben usar para validar el certificado de servidor, puede escribir el hash SHA-1 del certificado.

Este procedimiento muestra cómo obtener el valor SHA-1 de hash de un certificado de CA raíz de confianza utilizando el complemento Microsoft Management Console (MMC) de certificados. 

Para completar este procedimiento, debe ser miembro de la **usuarios** grupo en el equipo local.

### <a name="to-obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>Para obtener el valor SHA-1 de hash de un certificado de CA raíz de confianza

1. En el cuadro de diálogo Ejecutar o Windows PowerShell, escriba **mmc**, y, a continuación, presione ENTRAR. Microsoft Management Console \(MMC\) se abre. En MMC, haga clic en **archivo**, a continuación, haga clic en **Snap\in agregar o quitar**. Se abre el cuadro de diálogo **Agregar o quitar complementos**.

2. En **Agregar o quitar complementos**, en **Complementos disponibles**, haga doble clic en **Certificados**. Abre el Asistente del complemento certificados. Haga clic en **Cuenta de equipo** y, a continuación, en **Siguiente**.

3. En **Seleccionar equipo**, asegúrese de que **equipo Local (el equipo se está ejecutando esta consola)** está seleccionado, haga clic en **finalizar**y, a continuación, haga clic en **Aceptar**.

4. En el panel izquierdo, haga doble clic en **certificados (equipo Local)** y, a continuación, haga doble clic en el **entidades emisoras raíz de confianza** carpeta.

5. El **certificados** carpeta es una subcarpeta de la **entidades emisoras raíz de confianza** carpeta. Haga clic en la carpeta **Certificados**.

6. En el panel de detalles, busque el certificado para la entidad de certificación raíz de confianza. Haga doble clic en el certificado. Se abre el cuadro de diálogo **Certificado**.

7. En el cuadro de diálogo **Certificado**, haga clic en la pestaña **Detalles**.

8. En la lista de campos, desplácese y seleccione **huella digital**.

9. En el panel inferior, se muestra la cadena hexadecimal que es el hash SHA-1 de su certificado. Seleccione el hash SHA-1 y, a continuación, presione el método abreviado de teclado de Windows para el comando Copy \(CTRL\+C\) para copiar el valor hash en el Portapapeles de Windows.

10. Abra la ubicación a la que desea pegar el hash SHA-1, ubicar correctamente el cursor y, a continuación, presione el método abreviado de teclado de Windows para el comando Pegar \(CTRL\+V\). 

Para obtener más información sobre los certificados y NPS, consulte [configurar plantillas de certificados para PEAP y EAP requisitos](nps-manage-cert-requirements.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).

---
title: Obtener y configurar certificados de firma y descifrado de tokens de AD FS
description: Este documento describe cómo obtener y configurar el TS y TD certificados de AD FS
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/23/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d16a886c9fb8f88748ffe732a75f0fd6a32c3702
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820216"
---
# <a name="obtain-and-configure-ts-and-td-certificates-for-ad-fs"></a>Obtener y configurar TS y TD certificados de AD FS

Este tema describe las tareas y procedimientos que puede realizar para asegurarse de que los certificados de descifrado de tokens y de firma de tokens de AD FS están actualizados.

Certificados de firma de tokens son estándar X509 certificados que se usan para firmar de forma segura todos los tokens que emite el servidor de federación. Los certificados de descifrado de tokens son estándar X509 certificados que se usan para descifrar los tokens entrantes. Que se publican también en los metadatos de federación.

Para obtener más información, consulte [los requisitos de certificado](../design/ad-fs-requirements.md#BKMK_1)

## <a name="determine-whether-ad-fs-renews-the-certificates-automatically"></a>Determinar si AD FS renueva automáticamente los certificados
De forma predeterminada, AD FS está configurado para generar certificados de firma y descifrado de tokens automáticamente, en el momento de la configuración inicial y cuando los certificados se aproximan a su fecha de caducidad.

Puede ejecutar el siguiente comando de Windows PowerShell: `Get-AdfsProperties`.
  
  ![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts1.png)
  
La propiedad AutoCertificateRollover describe si AD FS está configurado para renovar el token automáticamente de los certificados de descifrado y firma de tokens.

Si AutoCertificateRollover está establecida en TRUE, los certificados de AD FS se renuevan y se configura automáticamente en AD FS. Una vez que se configura el nuevo certificado, con el fin de evitar una interrupción, debe asegurarse de que cada asociado de federación (representado en la granja de AD FS mediante relaciones de confianza o confianzas de proveedores de notificaciones) se actualiza con este nuevo certificado.
    
Si AD FS no está configurada para renovar el token de firma y el token de descifrado de certificados automáticamente (si AutoCertificateRollover está establecida en False), AD FS automáticamente no generarán o empezar a usar certificados de descifrado de tokens o firma de tokens de nuevo. Tendrá que realizar estas tareas manualmente.
    
Si AD FS está configurado para renovar el token automáticamente de los certificados de descifrado y firma de tokens (AutoCertificateRollover está establecido en TRUE), puede determinar cuándo se renovará:

CertificateGenerationThreshold describe cuántos días antes de la fecha del certificado después de no se generará un nuevo certificado.

CertificatePromotionThreshold determina cuántos días tras el nuevo certificado se genera que se promoverá para que sea el certificado principal (en otras palabras, AD FS iniciará usarlo para firmar los tokens que emite y descifrar los tokens de proveedores de identidades).

![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts2.png)
  
Si AD FS está configurado para renovar el token automáticamente de los certificados de descifrado y firma de tokens (AutoCertificateRollover está establecido en TRUE), puede determinar cuándo se renovará:

 - **CertificateGenerationThreshold** describe cuántos días antes de la fecha del certificado después de no se generará un nuevo certificado.
 - **CertificatePromotionThreshold** determina cuántos días tras el nuevo certificado se genera que se promoverá para que sea el certificado principal (en otras palabras, AD FS iniciará la usa para firmar los tokens que emite y descifrar los tokens de identidad proveedores).

## <a name="determine-when-the-current-certificates-expire"></a>Determinar cuándo expiran los certificados actuales
Puede usar el procedimiento siguiente para identificar la firma de tokens principal y los certificados de descifrado de token y para determinar cuándo expiran los certificados actuales.

Puede ejecutar el siguiente comando de Windows PowerShell: `Get-AdfsCertificate –CertificateType token-signing` (o `Get-AdfsCertificate –CertificateType token-decrypting `). O bien, puede examinar los certificados actuales en la consola MMC: Servicio -> certificados.

![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts3.png)

El certificado para el que el **IsPrimary** valor se establece en **True** es el certificado que está usando AD FS.

La fecha que aparece en el **no después** es la fecha por el que se debe configurar un nuevo token principal firmar o descifrar el certificado.

Para garantizar la continuidad del servicio, todos los socios de federación (representados en la granja de AD FS mediante relaciones de confianza o confianzas de proveedores de notificaciones) deben consumir los nuevos certificados de firma y descifrado de tokens antes de que esta expire. Se recomienda que empiece a planear para este proceso al menos 60 días de antelación.

## <a name="generating-a-new-self-signed-certificate-manually-prior-to-the-end-of-the-grace-period"></a>Generar un nuevo certificado autofirmado de forma manual antes del final del período de gracia
Puede usar los siguientes pasos para generar un nuevo certificado autofirmado de forma manual antes del final del período de gracia.

1. Asegúrese de que ha iniciado sesión el servidor de AD FS principal.
2. Abra Windows PowerShell y ejecute el siguiente comando: `Add-PSSnapin "microsoft.adfs.powershell"`
3. Si lo desea, puede comprobar los certificados de firmas actuales en AD FS. Para ello, ejecute el siguiente comando: `Get-ADFSCertificate –CertificateType token-signing`. Examine la salida del comando para ver las fechas no después de los certificados que se muestran.
4. Para generar un nuevo certificado, ejecute el siguiente comando para renovar y actualizar los certificados en el servidor de AD FS: `Update-ADFSCertificate –CertificateType token-signing`.
5. Comprobar la actualización de ejecutando de nuevo el comando siguiente: `Get-ADFSCertificate –CertificateType token-signing`
6. Ahora deben aparecer dos certificados, uno de los cuales tiene un **después de no** fecha de aproximadamente un año en el futuro y para los cuales el **IsPrimary** valor es **False**.

>[!IMPORTANT]
>Para evitar una interrupción del servicio, actualice la información del certificado en Azure AD mediante la ejecución de los pasos en el modo de actualización de Azure AD con un certificado de firma de tokens válido.

## <a name="if-youre-not-using-self-signed-certificates"></a>Si no usa los certificados autofirmados...
Si está utilizando no generado automáticamente de forma predeterminada, la firma de tokens autofirmado y certificados de descifrado de tokens, debe renovar y configurar estos certificados manualmente.

En primer lugar, debe obtener un nuevo certificado de la entidad de certificación e importarlo en el almacén de certificados personales del equipo local en cada servidor de federación. Para obtener instrucciones, consulte el [importar un certificado](https://technet.microsoft.com/library/cc754489.aspx) artículo.

A continuación, debe configurar este certificado como el secundario AD FS firma de tokens o certificado de descifrado. (Configura un certificado secundario para dar tiempo suficiente para consumir este nuevo certificado antes de pasar el certificado principal de sus socios de federación).

### <a name="to-configure-a-new-certificate-as-a-secondary-certificate"></a>Para configurar un nuevo certificado como certificado secundario
1. Abra PowerShell y ejecute lo siguiente: `Set-ADFSProperties -AutoCertificateRollover $false`
2. Una vez que ha importado el certificado. Abra el **administración de AD FS** consola.
3. Expanda **servicio** y, a continuación, seleccione **certificados**.
4. En el panel Acciones, haga clic en **certificado de firma de Token agregar**.
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts4.png)</br>
5. Seleccione el nuevo certificado en la lista de certificados que aparecen y, a continuación, haga clic en Aceptar.
6.  Abra PowerShell y ejecute lo siguiente: `Set-ADFSProperties -AutoCertificateRollover $true`

>[!WARNING]
>Asegúrese de que el nuevo certificado tiene una clave privada asociada con él y que la cuenta de servicio de AD FS se le concede permisos de lectura para la clave privada. Puede comprobar esto en cada servidor de federación. Para ello, en el complemento certificados, haga clic en el nuevo certificado, haga clic en todas las tareas y, a continuación, haga clic en administrar claves privadas.

Una vez que ha dejado suficiente tiempo para los socios de federación consumir el nuevo certificado (extraen los metadatos de federación o envío de la clave pública del nuevo certificado), debe promocionar el certificado secundario a certificado principal.

### <a name="to-promote-the-new-certificate-from-secondary-to-primary"></a>Para promover el nuevo certificado de secundario a principal

1. Abra el **administración de AD FS** consola.
2. Expanda **servicio** y, a continuación, seleccione **certificados**.
3. Haga clic en el certificado de firma de tokens secundario.
4. En el **acciones** panel, haga clic en **establecer como principal**. Haga clic en Sí en el mensaje de confirmación.
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts5.png)</br>


## <a name="updating-federation-partners"></a>Actualización de los socios de federación

### <a name="partners-who-can-consume-federation-metadata"></a>Asociados que pueden consumir los metadatos de federación
Si se ha renovado y configura un certificado de descifrado de tokens o firma de tokens de nuevo, debe asegurarse que el todas sus socios de federación (organización o cuenta de organización asociados de recurso que se representan en AD FS mediante el uso de confianzas de terceros y confianzas de proveedor de notificaciones) se recogen los nuevos certificados.

### <a name="partners-who-can-not-consume-federation-metadata"></a>Asociados que no se pueden consumir los metadatos de federación
Si los socios de federación no pueden consumir los metadatos de federación, debe manualmente los envía la clave pública del nuevo certificado firma de token y descifrado de tokens. Enviar la nueva clave pública del certificado (archivo .cer o. p7b si desea incluir toda la cadena) a todos los de su organización de recursos o los colaboradores de la organización (representados en AD FS mediante relaciones de confianza y confianzas de proveedores de notificaciones) de la cuenta. Tienen los asociados de implementar los cambios en su parte para los nuevos certificados de confianza.

### <a name="promote-to-primary-if-autocertificaterollover-is-false"></a>Promover a primario (si AutoCertificateRollover es False)
Si **AutoCertificateRollover** está establecido en **False**, AD FS no generará automáticamente o inicio con nuevas de firma de tokens o certificados de descifrado de tokens. Tendrá que realizar estas tareas manualmente.
Después de dejar un período de tiempo suficiente para todos los socios de federación para consumir el nuevo certificado secundario, promover este certificado secundario a principal (en el complemento de MMC, haga clic en la firma de tokens secundario de certificado y en el panel Acciones, haga clic en **Establecer como principal**.)

## <a name="updating-azure-ad"></a>Actualización de Azure AD
AD FS proporciona acceso de inicio de sesión único a los servicios en la nube de Microsoft como Office 365 mediante la autenticación de usuarios mediante sus credenciales de AD DS existentes.  Para obtener más información sobre el uso de certificados, consulte [renovar certificados de federación para Office 365 y Azure AD](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs).
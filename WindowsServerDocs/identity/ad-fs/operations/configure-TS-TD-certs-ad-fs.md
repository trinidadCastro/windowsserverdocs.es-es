---
title: Obtener y configurar la firma de tokens y los certificados de descifrado de Token de AD FS
description: "Este documento describe cómo obtener y configurar la TS y TD certificados de AD FS"
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/23/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0d7f8ba4efb74a377f9f9d748949a934398ebefc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="obtain-and-configure-ts-and-td-certificates-for-ad-fs"></a>Obtener y configurar TS y TD certificados de AD FS

Este tema describe los procedimientos que se pueden realizar para asegurarse de que la firma de tokens de AD FS y certificados de descifrado token estén actualizados y tareas.

Token de certificados de firma son estándar X509 certificados que se usan para firmar todos los tokens que emite el servidor de federación de forma segura. Los certificados de descifrado token son estándar X509 certificados que se usan para descifrar los tokens entrantes. También se publican en los metadatos de federación.

Para obtener más información, consulta [requisitos de certificados](../design/ad-fs-requirements.md#BKMK_1)

## <a name="determine-whether-ad-fs-renews-the-certificates-automatically"></a>Determinar si AD FS renueva automáticamente los certificados
De manera predeterminada, AD FS está configurado para generar la firma de tokens y certificados de descifrado token automáticamente, en el momento de la configuración inicial y cuando los certificados acercan su fecha de expiración.

Puedes ejecutar el siguiente comando de Windows PowerShell:`Get-AdfsProperties`.
  
  ![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts1.png)
  
La propiedad AutoCertificateRollover describe si AD FS está configurada para renovar el token de inicio de sesión y el token de descifrado certificados automáticamente.

Si AutoCertificateRollover se establece en TRUE, los certificados de AD FS se renuevan y se configura automáticamente en AD FS. Una vez configurado el nuevo certificado, con el fin de evitar una interrupción, debes asegurarte de que se actualiza cada socio de federación (representada en el conjunto de AD FS confianzas de terceros de confianza o reclamaciones proveedor confianzas) con este nuevo certificado.
    
Si AD FS no está configurado para renovar la firma de tokens y token descifrado certificados automáticamente (si AutoCertificateRollover se establece en "false"), AD FS no automáticamente generarán o empezar a usar la firma de tokens nuevo o token descifrado de certificados. Tendrás que realizar estas tareas manualmente.
    
Si AD FS está configurado para renovar el token de inicio de sesión y el token de descifrado certificados automáticamente (AutoCertificateRollover se establece en TRUE), puedes determinar cuándo se renovará:

CertificateGenerationThreshold describe cuántos días antes de la fecha del certificado no después de que se generará un nuevo certificado.

CertificatePromotionThreshold determina cuántos días después de nuevo certificado se genera de que se seleccionará para que sea el certificado principal (en otras palabras, AD FS iniciará usarla para iniciar sesión tokens emite y descifrar los tokens de proveedores de identidad).

![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts2.png)
  
Si AD FS está configurado para renovar el token de inicio de sesión y el token de descifrado certificados automáticamente (AutoCertificateRollover se establece en TRUE), puedes determinar cuándo se renovará:

 - **CertificateGenerationThreshold** describe cuántos días antes de la fecha del certificado no después de que se generará un nuevo certificado.
 - **CertificatePromotionThreshold** determina cuántos días después de nuevo el certificado se genera que se seleccionará para que sea el certificado principal (en otras palabras, AD FS iniciará usarla para iniciar sesión tokens emite y descifrar los tokens de identidad proveedores de).

## <a name="determine-when-the-current-certificates-expire"></a>Determinar cuándo caducan los certificados actuales
Puedes usar el siguiente procedimiento para identificar la firma de token principal y el token de descifrado de certificados y para determinar cuándo caducan los certificados actuales.

Puedes ejecutar el siguiente comando de Windows PowerShell: `Get-AdfsCertificate –CertificateType token-signing` (o `Get-AdfsCertificate –CertificateType token-decrypting `). O puedes examinar los certificados actuales en la consola MMC: servicio -> certificados.

![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts3.png)

El certificado para que la **IsPrimary** valor se establece en **True** es el certificado que AD FS está usando actualmente.

La fecha en que se muestra para la **no después** es la fecha en que debe configurarse un nuevo token principal firmar o decodificar certificado.

Para garantizar la continuidad del servicio, deben consumir todos los socios de federación (representados en el conjunto de AD FS confianzas de terceros de confianza o reclamaciones proveedor confianzas) los certificados de descifrado token antes esta fecha de expiración y la firma de tokens nuevo. Te recomendamos que empezar a diseñar para este proceso al menos 60 días de antelación.

## <a name="generating-a-new-self-signed-certificate-manually-prior-to-the-end-of-the-grace-period"></a>Generar un nuevo certificado autofirmado manualmente antes del final del período de gracia
Puedes usar los siguientes pasos para generar un nuevo certificado autofirmado manualmente antes del final del período de gracia.

1. Asegúrate de que has iniciado sesión en el servidor de AD FS principal.
2. Abre Windows PowerShell y ejecuta el siguiente comando: `Add-PSSnapin "microsoft.adfs.powershell"`
3. Opcionalmente, puedes comprobar los certificados de firma actuales en AD FS. Para ello, ejecuta el siguiente comando:`Get-ADFSCertificate –CertificateType token-signing`. Mira el resultado del comando para ver las fechas no después de los certificados que aparece.
4. Para generar un nuevo certificado, ejecute el siguiente comando para renovar y actualizar los certificados en el servidor de AD FS:`Update-ADFSCertificate –CertificateType token-signing`.
5. Para comprobar la actualización, volver a ejecutar el comando siguiente: `Get-ADFSCertificate –CertificateType token-signing`
6. Deberían aparecer ahora dos certificados, uno de los cuales tiene un **no después** fecha de aproximadamente un año en el futuro y para que la **IsPrimary** valor es **False**.

>[!IMPORTANT]
>Para evitar una interrupción del servicio, actualizar la información de certificado en Azure AD mediante la ejecución de los pasos de la actualización de Azure AD con un certificado de firma de tokens válido.

## <a name="if-youre-not-using-self-signed-certificates"></a>Si no usas certificados autofirmados...
Si se está utilizando no generado automáticamente de forma predeterminada, la firma de tokens autofirmado y certificados de descifrado de token, debes renovar y configurar estos certificados manualmente.

En primer lugar, debes obtener un nuevo certificado de la entidad de certificación e importarlo en el almacén de certificados personal del equipo local en cada servidor de federación. Para obtener instrucciones, consulta la [importar un certificado](https://technet.microsoft.com/library/cc754489.aspx) artículo.

A continuación, debes configurar este certificado como la secundaria AD FS token firma o descifrado. (Configurarla como un certificado secundario para permitir a los socios de federación tiempo suficiente para consumir este nuevo certificado antes de ascender al certificado principal).

### <a name="to-configure-a-new-certificate-as-a-secondary-certificate"></a>Para configurar un nuevo certificado como un certificado secundario
1. Abre PowerShell y ejecuta lo siguiente: `Set-ADFSProperties -AutoCertificateRollover $false`
2. Una vez ha importado el certificado. Abre el **AD FS administración** consola.
3. Expande **servicio** y, a continuación, selecciona **certificados**.
4. En el panel de acciones, haz clic en **certificado de firma de agregar Token**.
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts4.png)</br>
5. Selecciona el nuevo certificado de la lista de certificados mostrados y, a continuación, haz clic en Aceptar.
6.  Abre PowerShell y ejecuta lo siguiente: `Set-ADFSProperties -AutoCertificateRollover $true`

>[!WARNING]
>Asegúrate de que el nuevo certificado tenga una clave privada asociada a ella y que la cuenta de servicio de AD FS se concede permisos de lectura para la clave privada. Para comprobar esto en cada servidor de federación. Para ello, en el complemento certificados, haz clic en el nuevo certificado, haz clic en todas las tareas y, a continuación, haz clic en administrar claves privadas.

Una vez que has permitido tiempo suficiente para los socios de federación consumir el nuevo certificado (extraigan los metadatos de federación o enviarles la clave pública del certificado nuevo), debe promover el certificado secundario al certificado principal.

### <a name="to-promote-the-new-certificate-from-secondary-to-primary"></a>Para promover el nuevo certificado de secundario como principal

1. Abre el **AD FS administración** consola.
2. Expande **servicio** y, a continuación, selecciona **certificados**.
3. Haz clic en el certificado de firma de token secundario.
4. En la **acciones** panel, haz clic en **establecer como principal**. Haz clic en Sí en el aviso de confirmación.
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts5.png)</br>


## <a name="updating-federation-partners"></a>Actualización de federación de partners

### <a name="partners-who-can-consume-federation-metadata"></a>Asociados que pueden consumir federación metadatos
Si se ha renovado y configura una firma de tokens nuevo o un certificado de descifrado de token, debe asegurarse de que todos tus asociados federación (confianzas de terceros de la organización o cuenta de organización asociados de recurso que se representan en tu AD FS mediante confiar y los nuevos certificados han seleccionado reclamaciones confianzas de proveedor).

### <a name="partners-who-can-not-consume-federation-metadata"></a>Asociados que no se utilizan los metadatos de federación
Si los socios de federación no pueden consumir los metadatos de federación, debe manualmente los envías la clave pública de tu nuevo certificado de firma de tokens / descifrado de token. Enviar la nueva clave pública de certificado (.cer archivo o. p7b si desea incluir toda la cadena) para todos tus asociados de organización de organización o cuenta de recursos (representado en tu AD FS gracias a la parte confía y notificaciones de proveedor relaciones de confianza). Tienen los socios a implementar cambios en su lado para confiar en los certificados nuevo.

### <a name="promote-to-primary-if-autocertificaterollover-is-false"></a>Promocionar principal (si AutoCertificateRollover es "false")
Si **AutoCertificateRollover** se establece en **False**, AD FS no generará automáticamente o inicial con nuevo token de inicio de sesión o token descifrar certificados. Tendrás que realizar estas tareas manualmente.
Después de permitir un período de tiempo suficiente para todos tus asociados federación para consumir el nuevo certificado secundario, promover este certificado secundario principal (en el complemento de MMC, haga clic en la firma de tokens secundario de certificados y en el panel de acciones, haz clic en **Establecer como principal**.)

## <a name="updating-azure-ad"></a>Actualización de Azure AD
AD FS proporciona acceso de inicio de sesión único a los servicios de nube de Microsoft, como Office 365 al autenticar a los usuarios a través de sus credenciales de AD DS existentes.  Para obtener más información sobre el uso de certificados consulta [renovar los certificados de federación de Office 365 y Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-o365-certs).
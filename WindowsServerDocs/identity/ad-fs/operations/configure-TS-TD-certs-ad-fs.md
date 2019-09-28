---
title: Obtención y configuración de certificados de firma de tokens y descifrado de tokens para AD FS
description: En este documento se describe cómo obtener y configurar los certificados TS y TD para AD FS
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/23/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c544f956983357bdcaaaf2e5ee5b4ab5f80abb6e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407482"
---
# <a name="obtain-and-configure-ts-and-td-certificates-for-ad-fs"></a>Obtener y configurar certificados de TS y TD para AD FS

En este tema se describen las tareas y los procedimientos que puede realizar para asegurarse de que los certificados de firma de tokens y descifrado de tokens de AD FS están actualizados.

Los certificados de firma de tokens son certificados X509 estándar que se usan para firmar de forma segura todos los tokens que emite el servidor de Federación. Los certificados de descifrado de tokens son certificados X509 estándar que se usan para descifrar los tokens entrantes. También se publican en los metadatos de Federación.

Para obtener más información, consulte [requisitos de certificado](../design/ad-fs-requirements.md#BKMK_1)

## <a name="determine-whether-ad-fs-renews-the-certificates-automatically"></a>Determinar si AD FS se renuevan automáticamente los certificados
De forma predeterminada, AD FS está configurado para generar certificados de firma de tokens y descifrado de tokens automáticamente, tanto en el momento de la configuración inicial como cuando los certificados se aproximan a su fecha de expiración.

Puede ejecutar el siguiente comando de Windows PowerShell: `Get-AdfsProperties`.
  
  ![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts1.png)
  
La propiedad AutoCertificateRollover describe si AD FS está configurado para renovar automáticamente los certificados de firma de tokens y descifrado de tokens.

Si AutoCertificateRollover se establece en TRUE, los certificados de AD FS se renovarán y configurarán en AD FS automáticamente. Una vez configurado el nuevo certificado, para evitar una interrupción, debe asegurarse de que cada asociado de Federación (representado en la granja de AD FS mediante relaciones de confianza para usuario autenticado o confianzas de proveedor de notificaciones) se actualiza con este nuevo certificado.
    
Si AD FS no está configurado para renovar automáticamente los certificados de firma de tokens y descifrado de tokens (si AutoCertificateRollover está establecido en false), AD FS no generará ni comenzará automáticamente el uso de nuevos certificados de firma de tokens o descifrado de tokens. Tendrá que realizar estas tareas manualmente.
    
Si AD FS está configurado para renovar automáticamente los certificados de firma de tokens y descifrado de tokens (AutoCertificateRollover está establecido en TRUE), puede determinar cuándo se renovarán:

CertificateGenerationThreshold describe el número de días antes de la fecha en que se generará un certificado nuevo.

CertificatePromotionThreshold determina el número de días después de que se genere el nuevo certificado que se va a promocionar para que sea el certificado principal (es decir, AD FS comenzará a usarlo para firmar los tokens que emite y descifra los tokens de los proveedores de identidades).

![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts2.png)
  
Si AD FS está configurado para renovar automáticamente los certificados de firma de tokens y descifrado de tokens (AutoCertificateRollover está establecido en TRUE), puede determinar cuándo se renovarán:

 - **CertificateGenerationThreshold** describe el número de días antes de la fecha en que se generará un certificado nuevo.
 - **CertificatePromotionThreshold** determina el número de días después de que se genere el nuevo certificado que se va a promocionar para que sea el certificado principal (es decir, AD FS comenzará a usarlo para firmar los tokens que emite y descifra los tokens de la identidad. proveedores).

## <a name="determine-when-the-current-certificates-expire"></a>Determinar cuándo expiran los certificados actuales
Puede usar el procedimiento siguiente para identificar los certificados de firma y descifrado de tokens principales y para determinar cuándo expiran los certificados actuales.

Puede ejecutar el siguiente comando de Windows PowerShell: `Get-AdfsCertificate –CertificateType token-signing` (o `Get-AdfsCertificate –CertificateType token-decrypting `). O bien, puede examinar los certificados actuales en MMC: Certificados de > de servicio.

![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts3.png)

El certificado para el que el valor de **IsPrimary** se establece en **true** es el certificado que AD FS está usando actualmente.

La fecha que se muestra para el **no después** de es la fecha en la que debe configurarse un nuevo certificado de firma o descifrado de tokens primario.

Para garantizar la continuidad del servicio, todos los asociados de Federación (representados en la granja de AD FS mediante relaciones de confianza para usuario autenticado o de proveedor de notificaciones) deben usar los nuevos certificados de firma de tokens y descifrado de tokens antes de la expiración. Se recomienda que comience a planear este proceso al menos 60 días de antelación.

## <a name="generating-a-new-self-signed-certificate-manually-prior-to-the-end-of-the-grace-period"></a>Generar un nuevo certificado autofirmado de forma manual antes del final del período de gracia
Puede usar los pasos siguientes para generar un nuevo certificado autofirmado de forma manual antes del final del período de gracia.

1. Asegúrese de que ha iniciado sesión en el servidor de AD FS principal.
2. Abra Windows PowerShell y ejecute el siguiente comando:`Add-PSSnapin "microsoft.adfs.powershell"`
3. Opcionalmente, puede comprobar los certificados de firma actuales en AD FS. Para ello, ejecute el siguiente comando: `Get-ADFSCertificate –CertificateType token-signing`. Examine la salida del comando para ver las fechas no posteriores de los certificados enumerados.
4. Para generar un nuevo certificado, ejecute el siguiente comando para renovar y actualizar los certificados en el servidor de `Update-ADFSCertificate –CertificateType token-signing`AD FS:.
5. Ejecute el siguiente comando de nuevo para comprobar la actualización:`Get-ADFSCertificate –CertificateType token-signing`
6. Ahora deben aparecer dos certificados, uno de los cuales tiene una fecha **no después** de aproximadamente un año en el futuro y para el que el valor de **IsPrimary** es **false**.

>[!IMPORTANT]
>Para evitar una interrupción del servicio, actualice la información del certificado en Azure AD ejecutando los pasos descritos en la actualización de Azure AD con un certificado de firma de tokens válido.

## <a name="if-youre-not-using-self-signed-certificates"></a>Si no está usando certificados autofirmados...
Si no usa los certificados de firma y descifrado de tokens autofirmados y generados automáticamente, debe renovar y configurar estos certificados manualmente.

En primer lugar, debe obtener un nuevo certificado de la entidad de certificación e importarlo en el almacén de certificados personales del equipo local en cada servidor de Federación. Para obtener instrucciones, consulte el artículo sobre la [importación de un certificado](https://technet.microsoft.com/library/cc754489.aspx) .

A continuación, debe configurar este certificado como el certificado de descifrado o de firma de tokens de AD FS secundario. (Se configura como un certificado secundario para permitir que los asociados de Federación tengan suficiente tiempo para consumir este nuevo certificado antes de promoverlo al certificado principal).

### <a name="to-configure-a-new-certificate-as-a-secondary-certificate"></a>Para configurar un nuevo certificado como certificado secundario
1. Abra PowerShell y ejecute lo siguiente:`Set-ADFSProperties -AutoCertificateRollover $false`
2. Una vez que haya importado el certificado. Abra la consola de **Administración de AD FS** .
3. Expanda **servicio** y, a continuación, seleccione **certificados**.
4. En el panel acciones, haga clic en **Agregar certificado de firma de tokens**.
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts4.png)</br>
5. Seleccione el nuevo certificado en la lista de certificados que aparecen y, a continuación, haga clic en Aceptar.
6.  Abra PowerShell y ejecute lo siguiente:`Set-ADFSProperties -AutoCertificateRollover $true`

>[!WARNING]
>Asegúrese de que el nuevo certificado tiene una clave privada asociada y de que a la cuenta de servicio de AD FS se le conceden permisos de lectura para la clave privada. Compruébelo en cada servidor de Federación. Para ello, en el complemento certificados, haga clic con el botón secundario en el nuevo certificado, haga clic en todas las tareas y, a continuación, haga clic en administrar claves privadas.

Una vez que haya permitido el tiempo suficiente para que los asociados de Federación consuman el nuevo certificado (o bien extraen sus metadatos de Federación o les envía la clave pública del certificado nuevo), debe promover el certificado secundario al certificado principal.

### <a name="to-promote-the-new-certificate-from-secondary-to-primary"></a>Para promover el nuevo certificado de secundario a principal

1. Abra la consola de **Administración de AD FS** .
2. Expanda **servicio** y, a continuación, seleccione **certificados**.
3. Haga clic en el certificado de firma de tokens secundario.
4. En el panel **acciones** , haga clic en **establecer como principal**. Haga clic en sí en el mensaje de confirmación.
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts5.png)</br>


## <a name="updating-federation-partners"></a>Actualización de asociados de Federación

### <a name="partners-who-can-consume-federation-metadata"></a>Asociados que pueden consumir metadatos de Federación
Si ha renovado y configurado un nuevo certificado de firma de tokens o de descifrado de tokens, debe asegurarse de que todos los asociados de Federación (organización de recursos o asociados de la organización de cuentas representados en el AD FS por las relaciones de confianza para usuario autenticado y las confianzas de proveedor de notificaciones) han seleccionado los nuevos certificados.

### <a name="partners-who-can-not-consume-federation-metadata"></a>Asociados que no pueden consumir metadatos de Federación
Si los asociados de Federación no pueden usar los metadatos de Federación, debe enviarlos manualmente a la clave pública del nuevo certificado de firma de tokens/descifrado de tokens. Envíe la nueva clave pública de certificado (archivo. cer o. p7b si desea incluir toda la cadena) a todos los asociados de la organización de recursos o de la organización de cuentas (representados en el AD FS por las relaciones de confianza para usuario autenticado y las confianzas de proveedor de notificaciones). Haga que los asociados implementen los cambios en su lado para confiar en los nuevos certificados.

### <a name="promote-to-primary-if-autocertificaterollover-is-false"></a>Promover a principal (si AutoCertificateRollover es false)
Si **AutoCertificateRollover** se establece en **false**, AD FS no generará ni comenzará automáticamente el uso de nuevos certificados de firma de tokens o de descifrado de tokens. Tendrá que realizar estas tareas manualmente.
Después de permitir un período de tiempo suficiente para que todos los asociados de Federación consuman el nuevo certificado secundario, promueva este certificado secundario a principal (en el complemento MMC, haga clic en el certificado de firma de tokens secundario y, en el panel acciones, haga clic en **Establecer como principal**).

## <a name="updating-azure-ad"></a>Actualizar Azure AD
AD FS proporciona acceso de inicio de sesión único a los servicios en la nube de Microsoft, como Office 365, mediante la autenticación de los usuarios a través de sus credenciales de AD DS existentes.  Para obtener información adicional sobre el uso de certificados [, consulte renovación de certificados de Federación para Office 365 y Azure ad](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs).
---
ms.assetid: 4b71b212-7e5b-4fad-81ee-75b3d1f27869
title: Compatibilidad de AD FS con el enlace de nombre de host alternativo para la autenticación de certificado
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: be522f4dd990a920e910950c1bf2564d795bf9fa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358590"
---
# <a name="ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication"></a>Compatibilidad de AD FS con el enlace de nombre de host alternativo para la autenticación de certificado

En muchas redes, es posible que las directivas de Firewall local no permitan el tráfico a través de puertos no estándar como 49443. Esto se convirtió en un problema al intentar realizar la autenticación de certificados con AD FS antes de AD FS en Windows Server 2016. Esto se debe a que no puede tener enlaces diferentes para la autenticación de dispositivos y la autenticación de certificados de usuario en el mismo host. El puerto predeterminado 443 está enlazado para recibir certificados de dispositivo y no se puede modificar para que admita varios enlaces en el mismo canal. Los resultados fueron que la autenticación mediante tarjeta inteligente no funcionaba y los usuarios no eran conscientes de lo que ha sucedido, ya que no hay ninguna indicación de lo que ha sucedido realmente.  
  
Con AD FS en Windows Server 2016, esto puede realizarse.
  
En AD FS en Windows Server 2016, esto ha cambiado. Ahora se admiten dos modos, el primero usa el mismo host (es decir, adfs.contoso.com) con distintos puertos (443, 49443). El segundo usa hosts diferentes (adfs.contoso.com y certauth.adfs.contoso.com) con el mismo puerto (443). Para ello, se necesitará un certificado SSL para admitir "certauth. < ADFS-Service-Name >" como nombre de sujeto alternativo. Esto puede hacerse en el momento de la creación de la granja o posteriormente a través de PowerShell.  
  
## <a name="how-to-configure-alternate-host-name-binding-for-certificate-authentication"></a>Cómo configurar un enlace de nombre de host alternativo para la autenticación de certificados  
Hay dos maneras de agregar el enlace de nombre de host alternativo para la autenticación de certificados. La primera es cuando se configura una nueva granja de AD FS con AD FS para Windows Server 2016, si el certificado contiene un nombre alternativo del firmante (SAN), se configurará automáticamente para usar el segundo método mencionado anteriormente. Es decir, configurará automáticamente dos hosts diferentes (sts.contoso.com y certauth.sts.contoso.com con el mismo puerto. Si el certificado no contiene una SAN, verá una advertencia que indica que los nombres alternativos del firmante del certificado no admiten certauth. *. Vea las capturas de pantallas siguientes. El primero muestra una instalación donde el certificado tenía una SAN y el segundo muestra un certificado que no lo hizo.  
  
![enlace de nombre de host alternativo](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_1.png)  
  
![enlace de nombre de host alternativo](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_2.png)  
  
Del mismo modo, una vez que se haya implementado AD FS en Windows Server 2016, puede usar el cmdlet de PowerShell: Set-AdfsAlternateTlsClientBinding.
  
```powershell
Set-AdfsAlternateTlsClientBinding -Member DC1.contoso.com -Thumbprint '<thumbprint of cert>'
```

Cuando se le solicite, haga clic en sí para confirmar.  Y eso debe ser.

![enlace de nombre de host alternativo](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_3.png)

## <a name="additional-references"></a>Referencias adicionales

* [Administración de certificados SSL en AD FS y WAP en Windows Server 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

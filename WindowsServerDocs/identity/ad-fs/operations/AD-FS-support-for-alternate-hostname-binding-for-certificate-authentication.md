---
ms.assetid: 4b71b212-7e5b-4fad-81ee-75b3d1f27869
title: "AD FS soporte técnico para el enlace de nombre de host alternativo para la autenticación de certificado"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 553ff059693c7b0c0e6f0364d82c1adbca661097
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication"></a>AD FS soporte técnico para el enlace de nombre de host alternativo para la autenticación de certificado

>Se aplica a: Windows Server 2016

En todas las redes de las directivas de firewall local no pueden permitir el tráfico a través de los puertos no estándares como 49443. Esto se convirtió en un problema al intentar realizar la autenticación de certificado con AD FS antes de AD FS en Windows Server 2016. Esto es porque no podría tener diferentes enlaces para la autenticación de dispositivo y autenticación de certificado de usuario en el mismo host. El puerto 443 predeterminado está enlazado a recibir certificados de dispositivo y no se puede modificar para admitir varios enlace en el mismo canal. Fueron los resultados que no funcionaba la autenticación con tarjeta inteligente y los usuarios ya estaban constancia de qué ha ocurrido ya que no hay ninguna indicación de lo que realmente ocurrió.  
  
Con AD FS en Windows Server 2016 para ello.
  
En AD FS en Windows Server 2016 ha cambiado. Ahora se admiten dos modos, el primero usa el mismo host (es decir, adfs.contoso.com) con diferentes puertos (443, 49443). El segundo usa distintos hosts (adfs.contoso.com y certauth.adfs.contoso.com) con el mismo puerto (443). Esto requiere un certificado SSL para admitir la "certauth. < nombre de servicio adfs >" como nombre de sujeto alternativo. Esto puede hacerse en el momento de la creación de granja o más adelante mediante PowerShell.  
  
## <a name="how-to-configure-alternate-host-name-binding-for-certificate-authentication"></a>Cómo configurar el enlace de nombre de host alternativo para la autenticación de certificado  
Existen dos formas que puedes agregar el enlace de nombre de host alternativo para la autenticación de certificado. La primera es al configurar un nuevo conjunto de AD FS con AD FS de Windows Server 2016, si el certificado contiene un nombre alternativo del firmante (SAN), se colocará automáticamente el programa de instalación usa el segundo método mencionado anteriormente. Es decir, realizará automáticamente la instalación de dos hosts diferentes (sts.contoso.com y certauth.sts.contoso.com con el mismo puerto. Si el certificado contiene un SAN, verá una advertencia que indica que los nombres alternativos de firmante de certificado no admite certauth.*. Consulta las capturas de pantalla siguiente. La primera que muestra una instalación donde el certificado tenía un SAN y el segundo un certificado que no se muestra.  
  
![enlace de nombre de host alternativo](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_1.png)  
  
![enlace de nombre de host alternativo](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_2.png)  
  
De igual modo, cuando se haya implementado AD FS en Windows Server 2016 puedes usar el cmdlet de PowerShell: conjunto AdfsAlternateTlsClientBinding.
  
```powershell
Set-AdfsAlternateTlsClientBinding -Member DC1.contoso.com -Thumbprint '<thumbprint of cert>'
```

Cuando aparezca el mensaje, haz clic en Sí para confirmar.  Y que debe.

![enlace de nombre de host alternativo](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_3.png)

## <a name="additional-references"></a>Referencias adicionales

* [Administración de certificados SSL en AD FS y WAP en Windows Server 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

---
title: Errores de negociación, configuración de sesión y conexión de árbol
description: Presenta cómo solucionar los errores de negociación, configuración de sesión y conexión de árbol.
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 6b310f08757f100dfc005a631e54d0a58d61025c
ms.sourcegitcommit: 2365a7b23e2eccd13be350306c622d2ad9d36bc8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2020
ms.locfileid: "96787738"
---
# <a name="negotiate-session-setup-and-tree-connect-failures"></a>Errores de negociación, configuración de sesión y conexión de árbol

En este artículo se describe cómo solucionar los errores que se producen durante una solicitud de conexión de árbol, configuración de sesión y negociación de SMB.

## <a name="negotiate-fails"></a>Error de negociación

El servidor SMB recibe una solicitud de negociación SMB de un cliente SMB. Se agota el tiempo de espera de la conexión y se restablece después de 60 segundos. Puede haber un mensaje de confirmación tras aproximadamente 200 microsegundos.

Este problema suele deberse a un programa antivirus.

Si usa Windows Server 2008 R2, hay revisiones para este problema. Asegúrese de que el cliente SMB y el servidor SMB están actualizados.

## <a name="session-setup-fails"></a>Error de configuración de sesión

El servidor SMB recibe una \_ solicitud de configuración de sesión SMB de un cliente SMB pero no pudo responder.

Si el nombre de dominio completo (FQDN) o el nombre del sistema básico de entrada y salida (NetBIOS) del servidor es ' sed ' en la ruta de acceso de la Convención de nomenclatura universal (UNC), Windows utilizará Kerberos para la autenticación.

Después de la respuesta de negociación, se intentará obtener un vale de Kerberos para el nombre principal de servicio (SPN) del sistema de archivos de Internet común (CIFS) del servidor. Observe el tráfico Kerberos en el puerto TCP 88 para asegurarse de que no hay errores de Kerberos cuando el cliente SMB obtiene el token.

> [!NOTE]
> Los errores que se producen durante la autenticación previa de Kerberos son correctos. Los errores que se producen después de la autenticación previa de Kerberos (instancias en las que la autenticación no funciona) son los errores que causaron el problema de SMB.

Además, realice las siguientes comprobaciones:

- Examine el BLOB de seguridad en la solicitud de configuración de sesión SMB para asegurarse de que \_ se envían las credenciales correctas.

- Intente deshabilitar la protección del nombre del servidor SMB (**SmbServerNameHardeningLevel = 0**).

- Asegúrese de que el servidor SMB tiene un SPN cuando se tiene acceso a él a través de un registro DNS CNAME.

- Asegúrese de que la firma SMB funciona. (Esto es especialmente importante para dispositivos antiguos de terceros).

## <a name="tree-connect-fails"></a>Error de conexión de árbol

Asegúrese de que las credenciales de la cuenta de usuario tienen permisos de sistema de archivos NT y recursos compartidos (NTFS) en la carpeta.

La causa de los errores de conexión de árbol se puede encontrar en [3.3.5.7 recibir una \_ solicitud de conexión de árbol de SMB2](/openspecs/windows_protocols/ms-smb2/652e0c14-5014-4470-999d-b174d7b2da87). A continuación se muestran las soluciones para dos códigos de estado comunes.

\[ESTADO \_ \_ nombre de red incorrecto \_\]

Asegúrese de que el recurso compartido existe en el servidor y de que está escrito correctamente en la solicitud de cliente SMB.

\[Estado de \_ acceso \_ denegado\]

Compruebe que el disco y la carpeta utilizados por el recurso compartido existen y son accesibles.

Si usa SMBv3 o posterior, compruebe si el servidor y el recurso compartido requieren cifrado, pero el cliente no admite el cifrado. Para ello, realice estas acciones:

- Compruebe el servidor ejecutando el siguiente comando.

  ```PowerShell
  Get-SmbServerConfiguration | select Encrypt*
  ```

  Si EncryptData y RejectUnencryptedAccess son true, el servidor requiere cifrado.

- Para comprobar el recurso compartido, ejecute el siguiente comando:

  ```PowerShell
  Get-SmbShare | select name, EncryptData  
  ```

  Si EncryptData es true en el recurso compartido y RejectUnencryptedAccess es true en el servidor, el recurso compartido necesita el cifrado.

Siga estas instrucciones para solucionar el problema:

- Windows 8, Windows Server 2012 y las versiones posteriores de Windows admiten el cifrado del lado cliente (SMBv3 y versiones posteriores).

- Windows 7, Windows Server 2008 R2 y versiones anteriores de Windows no admiten el cifrado del lado cliente.

- Samba y el dispositivo de terceros pueden no admitir el cifrado. Es posible que tenga que consultar la documentación del producto para obtener más información.

## <a name="references"></a>Referencias

Para obtener más información, consulte los siguientes artículos.

[3.3.5.4 recibir una solicitud de negociación de SMB2](/openspecs/windows_protocols/ms-smb2/b39f253e-4963-40df-8dff-2f9040ebbeb1)

[3.3.5.5 recibir una solicitud de configuración de sesión de SMB2 \_](/openspecs/windows_protocols/ms-smb2/e545352b-9f2b-4c5e-9350-db46e4f6755e)

[3.3.5.7 recibir una solicitud de conexión de árbol de SMB2 \_](/openspecs/windows_protocols/ms-smb2/652e0c14-5014-4470-999d-b174d7b2da87)

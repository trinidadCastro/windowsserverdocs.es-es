---
title: Acerca del cifrado de volcado
description: Describe cómo cifrar archivos de volcado y solucionar problemas de cifrado.
manager: dongill
ms.topic: article
author: larsiwer
ms.asset: b78ab493-e7c3-41f5-ab36-29397f086f32
ms.author: kathydav
ms.date: 11/03/2016
ms.openlocfilehash: e80af001a54d3be471b3bbcc9fde08a07556d754
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87993437"
---
# <a name="about-dump-encryption"></a>Acerca del cifrado de volcado
El cifrado de volcado se puede usar para cifrar volcados de memoria y volcados en directo generados para un sistema. Los volcados de memoria se cifran mediante una clave de cifrado simétrica que se genera para cada volcado. Esta clave se cifra a continuación con la clave pública especificada por el administrador de confianza del host (protector de la clave de cifrado de volcado). Esto garantiza que solo alguien que tenga la clave privada correspondiente puede descifrar y, por tanto, tener acceso al contenido del volcado. Esta funcionalidad se aprovecha en un tejido protegido.
Nota: Si configura el cifrado de volcado, deshabilite también Informe de errores de Windows. WER no puede leer volcados de memoria cifrados.

## <a name="configuring-dump-encryption"></a>Configuración del cifrado de volcado
### <a name="manual-configuration"></a>Configuración manual
Para activar el cifrado de volcado mediante el registro, configure los siguientes valores del registro en`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl`

| Nombre del valor | Tipo | Valor |
| ---------- | ---- | ----- |
| DumpEncryptionEnabled | DWORD | 1 para habilitar el cifrado de volcado, 0 para deshabilitar el cifrado de volcado |
| EncryptionCertificates\Certificate.1::P ublicKey | Binary | Clave pública (RSA, 2048 bits) que se debe usar para cifrar volcados de memoria. Esto tiene que tener el formato [BCRYPT_RSAKEY_BLOB](/windows/win32/api/bcrypt/ns-bcrypt-bcrypt_rsakey_blob). |
| EncryptionCertificates\Certificate.1:: Thumbprint | String | Huella digital del certificado para permitir la búsqueda automática de la clave privada en el almacén de certificados local al descifrar un volcado de memoria. |


### <a name="configuration-using-script"></a>Configuración mediante script
Para simplificar la configuración, hay disponible un [script de ejemplo](https://github.com/Microsoft/Virtualization-Documentation/tree/live/hyperv-tools/DumpEncryption) para habilitar el cifrado de volcado en función de una clave pública de un certificado.

1. En un entorno de confianza: cree un certificado con una clave RSA de 2048 bits y exporte el certificado público.
2. En hosts de destino: importe el certificado público al almacén de certificados local.
3. Ejecutar el script de configuración de ejemplo
    ```
    .\Set-DumpEncryptionConfiguration.ps1 -Certificate (Cert:\CurrentUser\My\093568AB328DF385544FAFD57EE53D73EFAAF519) -Force
    ```

## <a name="decrypting-encrypted-dumps"></a>Descifrado de volcados de memoria
Para descifrar un archivo de volcado de memoria existente, debe descargar e instalar las herramientas de depuración para Windows. Este conjunto de herramientas contiene KernelDumpDecrypt.exe que se pueden usar para descifrar un archivo de volcado de memoria cifrado.
Si el certificado que incluye la clave privada está presente en el almacén de certificados del usuario actual, el archivo de volcado de memoria se puede descifrar mediante una llamada a

```
    KernelDumpDecrypt.exe memory.dmp memory_decr.dmp
```
Después del descifrado, las herramientas como WinDbg pueden abrir el archivo de volcado descifrado.

## <a name="troubleshooting-dump-encryption"></a>Solucionar problemas de cifrado de volcado
Si el cifrado de volcado está habilitado en un sistema pero no se genera ningún volcado, compruebe el registro de eventos del sistema en `System` busca del `Kernel-IO` evento 1207. Cuando no se puede inicializar el cifrado de volcado, se crea este evento y se deshabilitan los volcados.

| Mensajes de error detallados | Pasos para mitigar |
| ---------------------- | ----------------- |
| Falta el registro de la clave pública o huella digital | Compruebe si los dos valores del registro existen en la ubicación esperada |
| Clave pública no válida | Asegúrese de que la clave pública almacenada en el valor del registro PublicKey esté almacenada como [BCRYPT_RSAKEY_BLOB](/windows/win32/api/bcrypt/ns-bcrypt-bcrypt_rsakey_blob). |
| Tamaño de clave pública no compatible | Actualmente, solo se admiten claves RSA de 2048 bits. Configurar una clave que coincida con este requisito |

Compruebe también si el valor `GuardedHost` de `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled` se establece en un valor distinto de 0. Esto deshabilita los volcados de memoria por completo. Si es así, establézcalo en 0.
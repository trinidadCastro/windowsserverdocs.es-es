---
title: Acerca del cifrado de volcado
description: Describe cómo cifrar archivos de volcado y solucionar problemas de cifrado.
ms.prod: windows-server-threshold
manager: dongill
ms.topic: article
author: larsiwer
ms.asset: b78ab493-e7c3-41f5-ab36-29397f086f32
ms.author: kathydav
ms.date: 11/03/2016
ms.openlocfilehash: d46deee7fc9d911de2a6ee44ae097affe1d658a3
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872139"
---
# <a name="about-dump-encryption"></a>Acerca del cifrado de volcado
El cifrado de volcado se puede usar para cifrar volcados de memoria y volcados en directo generados para un sistema. Los volcados de memoria se cifran mediante una clave de cifrado simétrica que se genera para cada volcado. Esta clave se cifra a continuación con la clave pública especificada por el administrador de confianza del host (protector de la clave de cifrado de volcado). Esto garantiza que solo alguien que tenga la clave privada correspondiente puede descifrar y, por tanto, tener acceso al contenido del volcado. Esta funcionalidad se aprovecha en un tejido protegido.
Nota: Si configura el cifrado de volcado, deshabilite también Informe de errores de Windows. WER no puede leer volcados de memoria cifrados.

# <a name="configuring-dump-encryption"></a>Configuración del cifrado de volcado
## <a name="manual-configuration"></a>Configuración manual
Para activar el cifrado de volcado mediante el registro, configure los siguientes valores del registro en`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl`

| Nombre del valor | Type | Valor |
| ---------- | ---- | ----- |
| DumpEncryptionEnabled | DWORD | 1 para habilitar el cifrado de volcado, 0 para deshabilitar el cifrado de volcado |
| EncryptionCertificates\Certificate.1::P ublicKey | Binary | Clave pública (RSA, 2048 bits) que se debe usar para cifrar volcados de memoria. Este debe tener el formato [BCRYPT_RSAKEY_BLOB](https://msdn.microsoft.com/library/windows/desktop/aa375531(v=vs.85).aspx). |
| EncryptionCertificates\Certificate.1:: Thumbprint | string | Huella digital del certificado para permitir la búsqueda automática de la clave privada en el almacén de certificados local al descifrar un volcado de memoria. |


## <a name="configuration-using-script"></a>Configuración mediante script
Para simplificar la configuración, hay disponible un [script de ejemplo](https://github.com/Microsoft/Virtualization-Documentation/tree/live/hyperv-tools/DumpEncryption) para habilitar el cifrado de volcado en función de una clave pública de un certificado.

1. En un entorno de confianza: Cree un certificado con una clave RSA de 2048 bits y exporte el certificado público.
2. En hosts de destino: Importar el certificado público en el almacén de certificados local
3. Ejecutar el script de configuración de ejemplo 
    ```
    .\Set-DumpEncryptionConfiguration.ps1 -Certificate (Cert:\CurrentUser\My\093568AB328DF385544FAFD57EE53D73EFAAF519) -Force
    ```

# <a name="decrypting-encrypted-dumps"></a>Descifrado de volcados de memoria
Para descifrar un archivo de volcado de memoria existente, debe descargar e instalar las herramientas de depuración para Windows. Este conjunto de herramientas contiene KernelDumpDecrypt. exe, que se puede usar para descifrar un archivo de volcado de memoria cifrado.
Si el certificado que incluye la clave privada está presente en el almacén de certificados del usuario actual, el archivo de volcado de memoria se puede descifrar mediante una llamada a

```
    KernelDumpDecrypt.exe memory.dmp memory_decr.dmp
```
Después del descifrado, las herramientas como WinDbg pueden abrir el archivo de volcado descifrado.

# <a name="troubleshooting-dump-encryption"></a>Solucionar problemas de cifrado de volcado
Si el cifrado de volcado está habilitado en un sistema pero no se genera ningún volcado, compruebe el `System` registro de eventos `Kernel-IO` del sistema en busca del evento 1207. Cuando no se puede inicializar el cifrado de volcado, se crea este evento y se deshabilitan los volcados.

| Mensaje de error detallado | Pasos para mitigar |
| ---------------------- | ----------------- |
| Falta el registro de la clave pública o huella digital | Compruebe si los dos valores del registro existen en la ubicación esperada |
| Clave pública no válida | Asegúrese de que la clave pública almacenada en el valor del registro PublicKey esté almacenada como [BCRYPT_RSAKEY_BLOB](https://msdn.microsoft.com/library/windows/desktop/aa375531(v=vs.85).aspx). |
| Tamaño de clave pública no compatible | Actualmente, solo se admiten claves RSA de 2048 bits. Configurar una clave que coincida con este requisito |

Compruebe también si el valor `GuardedHost` de `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled` se establece en un valor distinto de 0. Esto deshabilita los volcados de memoria por completo. Si es así, establézcalo en 0.

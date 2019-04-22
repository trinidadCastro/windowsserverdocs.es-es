---
title: Acerca del cifrado de volcado de memoria
description: Describe cómo cifrar los archivos de volcado de memoria y solucionar problemas de cifrado.
ms.prod: windows-server-threshold
manager: dongill
ms.topic: article
author: larsiwer
ms.asset: b78ab493-e7c3-41f5-ab36-29397f086f32
ms.author: kathydav
ms.date: 11/03/2016
ms.openlocfilehash: e0ca8829aa8f93e7543b4183539beade3b4aeb47
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812396"
---
# <a name="about-dump-encryption"></a>Acerca del cifrado de volcado de memoria
Cifrado de volcado de memoria puede usarse para cifrar los volcados de memoria y volcados de memoria en vivo generados para un sistema. Los volcados de memoria se cifran mediante una clave de cifrado simétrico que se genera para cada descarga. Esta clave en Sí, a continuación, se cifra mediante la clave pública especificada por el Administrador de confianza del host (protector de clave crash dump cifrado). Esto garantiza que solo un usuario tiene la clave privada correspondiente puede descifrar y, por tanto, tener acceso a contenido del volcado de memoria. Se aprovecha esta capacidad en un tejido protegido.
Nota: Si configura el cifrado de volcado de memoria, también deshabilitar informe de errores de Windows. WER no puede leer el cifrado los volcados de memoria.

# <a name="configuring-dump-encryption"></a>Configurar el cifrado de volcado de memoria
## <a name="manual-configuration"></a>Configuración manual
Para activar cifrado de volcado de memoria mediante el registro, configure los siguientes valores del registro en `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl`

| Nombre del valor | Tipo | Valor |
| ---------- | ---- | ----- |
| DumpEncryptionEnabled | DWORD | 1 para habilitar el cifrado de volcado de memoria, 0 para deshabilitar el cifrado de volcado de memoria |
| EncryptionCertificates\Certificate.1::PublicKey | Binary | Clave pública (RSA, 2048 bits) que se debe usar para cifrar los volcados de memoria. Esto tiene que tener el formato [BCRYPT_RSAKEY_BLOB](https://msdn.microsoft.com/library/windows/desktop/aa375531(v=vs.85).aspx). |
| EncryptionCertificates\Certificate.1::Thumbprint | Cadena | Huella digital de certificado para permitir la búsqueda automática de la clave privada en el almacén de certificados local al descifrar un archivo de volcado. |


## <a name="configuration-using-script"></a>Configuración mediante la secuencia de comandos
Para simplificar la configuración, un [script de ejemplo](https://github.com/Microsoft/Virtualization-Documentation/tree/live/hyperv-tools/DumpEncryption) está disponible para habilitar el cifrado de volcado de memoria en función de una clave pública de un certificado.

1. En un entorno de confianza: Crear un certificado con una clave RSA de 2048 bits y exporte el certificado público
2. En los hosts de destino: Importe el certificado público para el almacén de certificados local
3. Ejecute el script de configuración de ejemplo 
    ```
    .\Set-DumpEncryptionConfiguration.ps1 -Certificate (Cert:\CurrentUser\My\093568AB328DF385544FAFD57EE53D73EFAAF519) -Force
    ```

# <a name="decrypting-encrypted-dumps"></a>Descifrado de los volcados de cifrado
Para descifrar un archivo de volcado cifrado existente, deberá descargar e instalar las herramientas de depuración para Windows. Este conjunto de herramientas contiene KernelDumpDecrypt.exe que puede usarse para descifrar un archivo de volcado de memoria cifrada.
Si el certificado, incluida la clave privada está presente en el almacén de certificados del usuario actual, se puede descifrar el archivo de volcado de memoria mediante una llamada a

```
    KernelDumpDecrypt.exe memory.dmp memory_decr.dmp
```
Después del descifrado, herramientas como WinDbg pueden abrir el archivo de volcado descifrada.

# <a name="troubleshooting-dump-encryption"></a>Solución de problemas de cifrado de volcado de memoria
Si está habilitado el cifrado de volcado de memoria en un sistema, pero no hay volcados de memoria que se generan, compruebe el sistema `System` registro de eventos para `Kernel-IO` evento 1207. Cuando no se puede inicializar el cifrado de volcado de memoria, este evento se crea y se deshabilitan los volcados de memoria.

| Mensaje de error detallado | Pasos para mitigar |
| ---------------------- | ----------------- |
| Pública huella digital o clave del registro que faltan | Comprobar si existen ambos valores del registro en la ubicación esperada |
| Clave pública no válida | Asegúrese de que la clave pública almacenada en el valor de clave pública del registro se almacena como [BCRYPT_RSAKEY_BLOB](https://msdn.microsoft.com/library/windows/desktop/aa375531(v=vs.85).aspx). |
| Tamaño de clave pública no admitido | Actualmente, las claves RSA de 2048 bits sola se admiten. Configurar una clave que coincida con este requisito |

Compruebe también si el valor `GuardedHost` en `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled` se establece en un valor distinto de 0. Esto deshabilita completamente los volcados de memoria. Si esto es así, establézcalo en 0.

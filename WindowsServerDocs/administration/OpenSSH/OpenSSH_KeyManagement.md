---
title: Administración de claves de OpenSSH para Windows
description: Administración de claves del servidor de OpenSSH para Windows con las herramientas de Windows o PowerShell.
ms.date: 09/27/2018
ms.topic: conceptual
ms.author: damaerte
author: maertendmsft
ms.openlocfilehash: c722b9fb81dde8e695d6065d7b2268f862e71d39
ms.sourcegitcommit: 00406560a665a24d5a2b01c68063afdba1c74715
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91716886"
---
# <a name="openssh-key-management"></a>Administración de claves de OpenSSH

La mayor parte de la autenticación en entornos de Windows se realiza con un par de nombre de usuario y contraseña.
Esta funciona bien en los sistemas que comparten un dominio común.
Al trabajar con varios dominios (por ejemplo, entre sistemas locales y hospedados en la nube), se expone más a intrusiones de fuerza bruta.

En comparación, los entornos de Linux, normalmente, usan pares de clave pública y privada para controlar la autenticación, lo que no requiere el uso de contraseñas que se puedan adivinar.
OpenSSH incluye herramientas que ayudan a admitir esta opción, en concreto:

* __ssh-keygen__ para generar claves seguras.
* __ssh-agent__ y __ssh-add__ para almacenar claves privadas de forma segura.
* __scp__ y __sftp__ para copiar archivos de claves públicas de forma segura durante el uso inicial de un servidor.

En este documento se proporciona información general sobre cómo usar estas herramientas en Windows para empezar a usar la autenticación de claves con SSH.
Si no estás familiarizado con la administración de claves SSH, se recomienda que revises el [informe interno de NIST 7966](http://nvlpubs.nist.gov/nistpubs/ir/2015/NIST.IR.7966.pdf) titulado "Seguridad de la administración de acceso interactiva y automatizada con Secure Shell (SSH)".

## <a name="about-key-pairs"></a>Acerca de los pares de claves

Los pares de claves hacen referencia a los archivos de clave pública y privada que utilizan determinados protocolos de autenticación.

La autenticación de clave pública SSH usa algoritmos criptográficos asimétricos para generar dos archivos de clave: uno "privado" y otro "público". Los archivos de clave privada son el equivalente de una contraseña y deben estar protegidos en todo momento. Si alguien adquiere tu clave privada, esa persona puede iniciar sesión en cualquier servidor SSH al que tengas acceso. La clave pública es la que se coloca en el servidor SSH y puede compartirse sin poner en peligro la clave privada.

Al usar la autenticación de claves con un servidor SSH, el servidor SSH y el cliente comparan las claves públicas del nombre de usuario proporcionado con la clave privada. Si la clave pública del lado servidor no se puede validar con la clave privada del lado cliente, se produce un error de autenticación.

Para implementar la autenticación multifactor con pares de claves, debes solicitar que se proporcione una frase de contraseña cuando se genera el par de claves (consulta Generación de claves a continuación).
Durante la autenticación, se solicita al usuario la frase de contraseña, que se usa junto con la presencia de la clave privada en el cliente SSH para autenticar al usuario.

## <a name="host-key-generation"></a>Generación de claves de host

Las claves públicas tienen requisitos de ACL específicos que, en Windows, equivalen a permitir el acceso únicamente a los administradores y al sistema.
Para facilitar esta tarea,

* El módulo de PowerShell de OpenSSHUtils se creó para establecer las ACL de claves correctamente y debe instalarse en el servidor.
* Al usar sshd por primera vez, se generará automáticamente el par de claves para el host. Si el ssh-agent se está ejecutando, las claves se agregarán automáticamente al almacén local.

Para facilitar la autenticación de claves con un servidor SSH, ejecuta los siguientes comandos desde un símbolo del sistema de PowerShell con privilegios elevados:

```powershell

# Install the OpenSSHUtils module to the server. This will be valuable when deploying user keys.
Install-Module -Force OpenSSHUtils -Scope AllUsers

# Start the ssh-agent service to preserve the server keys
Start-Service ssh-agent

# Now start the sshd service
Start-Service sshd
```

Dado que no hay ningún usuario asociado al servicio sshd, las claves de host se almacenan en \ProgramData\ssh.

## <a name="user-key-generation"></a>Generación de claves de usuario

Para usar la autenticación basada en claves, primero debes generar algunos pares de claves públicas o privadas para el cliente.
Desde PowerShell o cmd, usa ssh-keygen para generar algunos archivos de clave.

```powershell
cd ~\.ssh\
ssh-keygen
```

Esta operación debería mostrar algo similar a lo siguiente (donde "username" se reemplaza por tu nombre de usuario)

```
Generating public/private ed25519 key pair.
Enter file in which to save the key (C:\Users\username\.ssh\id_ed25519):
```

Puedes presionar Entrar para aceptar el valor predeterminado o puedes especificar una ruta de acceso en la que quieres que se generen las claves.
Llegados a este punto, se te pedirá que uses una frase de contraseña para cifrar los archivos de clave privada.
La frase de contraseña funciona con el archivo de clave para proporcionar una autenticación en dos fases.
En este ejemplo, vamos a dejar la frase de contraseña vacía.

```
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\username\.ssh\id_ed25519.
Your public key has been saved in C:\Users\username\.ssh\id_ed25519.pub.
The key fingerprint is:
SHA256:OIzc1yE7joL2Bzy8!gS0j8eGK7bYaH1FmF3sDuMeSj8 username@server@LOCAL-HOSTNAME

The key's randomart image is:
+--[ED25519 256]--+
|        .        |
|         o       |
|    . + + .      |
|   o B * = .     |
|   o= B S .      |
|   .=B O o       |
|  + =+% o        |
| *oo.O.E         |
|+.o+=o. .        |
+----[SHA256]-----+
```

Ahora tienes un par de claves ED25519 pública y privada (los archivos .pub son claves públicas y los demás son claves privadas):

```
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/28/2018  11:09 AM           1679 id_ed25519
-a----        9/28/2018  11:09 AM            414 id_ed25519.pub
```

Recuerda que los archivos de clave privada son el equivalente de una contraseña, por lo que se deben proteger de la misma manera que las contraseñas.
Para que sea más fácil, usa ssh-agent para almacenar de forma segura las claves privadas en un contexto de seguridad de Windows y asócialas con tu inicio de sesión de Windows.
Para ello, inicia el servicio de ssh-agent como administrador y usa ssh-add para almacenar la clave privada.

```powershell
# Make sure you're running as an Administrator
Start-Service ssh-agent

# This should return a status of Running
Get-Service ssh-agent

# Now load your key files into ssh-agent
ssh-add ~\.ssh\id_ed25519

```

Después de completar estos pasos, cada vez que se necesite una clave privada para la autenticación desde este cliente, ssh-agent recuperará automáticamente la clave privada local y la pasará al cliente SSH.

> [!NOTE]
> Se recomienda realizar una copia de seguridad de la clave privada en una ubicación segura y, a continuación, eliminarla del sistema local *después* de agregarla al agente SSH.
> No se puede recuperar la clave privada del agente.
> Si pierdes el acceso a la clave privada, tendrás que crear un nuevo par de claves y actualizar la clave pública en todos los sistemas con los que interactúes.

## <a name="deploying-the-public-key"></a>Implementación de la clave pública

Para usar la clave de usuario que se creó anteriormente, la clave pública debe colocarse en el servidor en un archivo de texto denominado *authorized_keys* en users\nombre de usuario\\.ssh\\.
Las herramientas de OpenSSH incluyen scp (una utilidad de transferencia de archivos segura) para ayudarte con esta operación.

Para trasladar el contenido de la clave pública (~\.ssh\id_ed25519.pub) a un archivo de texto denominado authorized_keys en ~\.ssh\ en el servidor o host.

En este ejemplo se usa la función Repair-AuthorizedKeyPermissions en el módulo OpenSSHUtils que se instaló anteriormente en el host de las instrucciones anteriores.

```powershell
# Make sure that the .ssh directory exists in your server's home folder
ssh user1@domain1@contoso.com mkdir C:\users\user1\.ssh\

# Use scp to copy the public key file generated previously to authorized_keys on your server
scp C:\Users\user1\.ssh\id_ed25519.pub user1@domain1@contoso.com:C:\Users\user1\.ssh\authorized_keys

# Appropriately ACL the authorized_keys file on your server
ssh --% user1@domain1@contoso.com powershell -c $ConfirmPreference = 'None'; Repair-AuthorizedKeyPermission C:\Users\user1\.ssh\authorized_keys
```

Estos pasos completan la configuración necesaria para usar la autenticación con SSH basada en claves en Windows.
Después de esto, el usuario puede conectarse al host de sshd desde cualquier cliente que tenga la clave privada.

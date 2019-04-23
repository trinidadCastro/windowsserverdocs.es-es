---
ms.date: 09/27/2018
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, instalar, configurar
contributor: maertendMSFT
author: maertendMSFT
title: Configuración del servidor de OpenSSH para Windows
ms.product: w10
ms.openlocfilehash: 3e2981cbbdfe34bf28a77d5121bff0b663e0d3bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837156"
---
# <a name="openssh-key-management"></a>Administración de clave OpenSSH

La mayoría de autenticación en entornos de Windows se realiza con un par de nombre de usuario y contraseña.
Esto funciona bien para los sistemas que comparten un dominio común. Cuando se trabaja en dominios, como entre sistemas locales y hospedadas en nube, resulta más difícil.

En comparación, los entornos Linux suelen usan pares de clave pública y clave privada a la autenticación de la unidad.
OpenSSH incluye herramientas para ayudar a admitir esto, específicamente:

* __SSH-keygen__ para generar las claves seguras
* __SSH-agent__ y __ssh-agregar__ para almacenar de forma segura las claves privadas
* __SCP__ y __sftp__ para copiar los archivos de clave públicos de forma segura durante el uso inicial de un servidor

Este documento proporciona información general sobre cómo usar estas herramientas en Windows para empezar a usar la autenticación de clave con SSH. Si no está familiarizado con administración de claves SSH, se recomienda que revise [documento del NIST IR 7966](http://nvlpubs.nist.gov/nistpubs/ir/2015/NIST.IR.7966.pdf) titulado "Seguridad de modo interactivo y automatizada Access Management Using Secure Shell (SSH)".

## <a name="about-key-pairs"></a>Acerca de los pares de claves

Pares de claves hacen referencia a los archivos de clave públicos y privados que se usan por determinados protocolos de autenticación. 

La autenticación de clave pública SSH utiliza algoritmos criptográficos asimétricos para generar dos archivos de claves: una "privada" y los otro segmentos "public". Los archivos de clave privados son el equivalente de una contraseña y deben proteger en todas las circunstancias. Si alguien obtiene la clave privada, puede iniciar sesión como a cualquier servidor SSH que tiene acceso. La clave pública es lo que se coloca en el servidor SSH y puede compartirse sin poner en peligro la clave privada.

Cuando se usa la autenticación de clave con un servidor SSH, el servidor SSH y el cliente comparan la clave pública para el nombre de usuario proporcionado con la clave privada. Si no se puede validar la clave pública con la clave privada del lado cliente, se produce un error de autenticación. 

Autenticación multifactor puede implementarse con pares de claves al requerir que se proporcione una frase de contraseña cuando se genera el par de claves (consulte la siguiente generación de claves). Durante la autenticación que se solicitará al usuario la frase de contraseña, que se usa junto con la presencia de la clave privada en el cliente SSH para autenticar al usuario. 

## <a name="host-key-generation"></a>Generación de claves de host

Las claves públicas tienen requisitos específicos de ACL que, en Windows, equivalen a sólo permita el acceso a los administradores y del sistema. Para facilitar esta tarea, 

* El módulo OpenSSHUtils PowerShell se ha creado para establecer la ACL de la clave correctamente y debe instalarse en el servidor
* En el primer uso de sshd, se generará automáticamente el par de claves para el host. Si se está ejecutando ssh-agent, las claves se agregará automáticamente al almacén local. 

Para facilitar la autenticación de clave con un servidor SSH, ejecute los siguientes comandos desde un símbolo del sistema con privilegios elevados de PowerShell:

```powershell

# Install the OpenSSHUtils module to the server. This will be valuable when deploying user keys.
Install-Module -Force OpenSSHUtils -Scope AllUsers

# Start the ssh-agent service to preserve the server keys
Start-Service ssh-agent

# Now start the sshd service
Start-Service sshd
```

Dado que no hay ningún usuario asociado con el servicio sshd, las claves de host se almacenan en \ProgramData\ssh.


## <a name="user-key-generation"></a>Generación de claves de usuario

Para usar la autenticación basada en claves, primero deberá generar algunos pares de claves pública y privada para el cliente. Desde PowerShell o cmd, use ssh-keygen para generar algunos archivos de clave.

```powershell
cd ~\.ssh\
ssh-keygen
```

Esto debería mostrar algo parecido a lo siguiente (donde "username" se sustituye por su nombre de usuario)

```
Generating public/private ed25519 key pair.
Enter file in which to save the key (C:\Users\username\.ssh\id_ed25519):
```

Puede presione ENTRAR para aceptar el valor predeterminado o especifique una ruta de acceso donde desea que la generación de claves. En este momento, se le pedirá que use una frase de contraseña para cifrar los archivos de clave privados.
La frase de contraseña funciona con el archivo de clave para proporcionar autenticación de factor de 2. En este ejemplo, abandonamos la frase de contraseña vacía. 

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

Ahora que tiene un par de claves del ED25519 del par de claves (los archivos. pub son las claves públicas y el resto son las claves privadas):

```
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/28/2018  11:09 AM           1679 id_ed25519
-a----        9/28/2018  11:09 AM            414 id_ed25519.pub
```

Recuerde que los archivos de clave privada son que el equivalente de una contraseña debe protegerse de la misma manera que proteger la contraseña.
Para ayudarle a, use ssh-agent para almacenar de forma segura las claves privadas dentro de un contexto de seguridad de Windows asociada con el inicio de sesión de Windows. Para ello, inicie el servicio de ssh-agent como administrador y use ssh-add para almacenar la clave privada. 

```powershell
# Make sure you're running as an Administrator
Start-Service ssh-agent

# This should return a status of Running
Get-Service ssh-agent

# Now load your key files into ssh-agent
ssh-add ~\.ssh\id_ed25519

```

Después de completar estos pasos, cada vez que se necesita una clave privada para la autenticación de este cliente, ssh-agent automáticamente recuperar la clave privada local y pasarlo a su cliente SSH.

> [!NOTE]
> Se recomienda encarecidamente que realizar una copia de seguridad de la clave privada en una ubicación segura, a continuación, eliminarlo del sistema local, *después* agregándolo a ssh-agent.
> No se puede recuperar la clave privada del agente.
> Si pierde el acceso a la clave privada, tendría que crear un nuevo par de claves y actualizar la clave pública en todos los sistemas que interactúa.

## <a name="deploying-the-public-key"></a>Implementación de la clave pública

Para usar la clave de usuario que se creó anteriormente, la clave pública debe ubicarse en el servidor en un archivo de texto denominado *authorized_keys* en users\username\ssh. Las herramientas de OpenSSH incluyen scp, que es una utilidad de transferencia de archivos seguro, para ayudar con esto.

Para mover el contenido de la clave pública (~\.ssh\id_ed25519.pub) en el texto de un archivo denominado authorized_keys ~\.ssh\ en el host de servidor.

En este ejemplo se usa la función de reparación AuthorizedKeyPermissions en el módulo OpenSSHUtils que se instaló previamente en el host en las instrucciones anteriores.

```powershell
# Make sure that the .ssh directory exists in your server's home folder
ssh user1@domain1@contoso.com mkdir C:\users\user1\.ssh\

# Use scp to opy the public key file generated previously to authorized_keys on your server
scp C:\Users\user1\.ssh\id_ed25519.pub user1@domain1@contoso.com:C:\Users\user1\.ssh\authorized_keys

# Appropriately ACL the authorized_keys file on your server  
ssh --% user1@domain1@contoso.com powershell -c $ConfirmPreference = 'None'; Repair-AuthorizedKeyPermission C:\Users\user1\.ssh\authorized_keys
```

Estos pasos completen la configuración necesaria para usar la autenticación basada en claves con SSH en Windows.
Una vez hecho esto, el usuario puede conectarse al host sshd desde cualquier cliente que tiene la clave privada.


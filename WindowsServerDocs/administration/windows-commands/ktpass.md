---
title: ktpass
description: Tema de referencia del comando ktpass, que configura el nombre de la entidad de seguridad del servidor para el host o el servicio en AD DS y genera un archivo. clave de claves que contiene la clave secreta compartida del servicio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 47087676-311e-41f1-8414-199740d01444
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 432918343ccee70f0c30d294a349fb721f18f705
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817235"
---
# <a name="ktpass"></a>ktpass

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Configura el nombre de la entidad de seguridad del servidor para el host o servicio en Active Directory Domain Services (AD DS) y genera un archivo. la clave de acceso que contiene la clave secreta compartida del servicio. El archivo .keytab se basa en la implementación del Instituto Tecnológico de Massachusetts (MIT) del protocolo de autenticación Kerberos. La herramienta de línea de comandos ktpass permite que los servicios que no son de Windows que admiten la autenticación Kerberos utilicen las características de interoperabilidad proporcionadas por el servicio Centro de distribución de claves de Kerberos (KDC).

## <a name="syntax"></a>Sintaxis

```
ktpass
[/out <filename>]
[/princ <principalname>]
[/mapuser <useraccount>]
[/mapop {add|set}] [{-|+}desonly] [/in <filename>]
[/pass {password|*|{-|+}rndpass}]
[/minpass]
[/maxpass]
[/crypto {DES-CBC-CRC|DES-CBC-MD5|RC4-HMAC-NT|AES256-SHA1|AES128-SHA1|All}]
[/itercount]
[/ptype {KRB5_NT_PRINCIPAL|KRB5_NT_SRV_INST|KRB5_NT_SRV_HST}]
[/kvno <keyversionnum>]
[/answer {-|+}]
[/target]
[/rawsalt] [{-|+}dumpsalt] [{-|+}setupn] [{-|+}setpass <password>]  [/?|/h|/help]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ------------|
| /out`<filename>` | Especifica el nombre del archivo de claves de claves de Kerberos versión 5 que se va a generar. **Nota:** Este es el archivo. la forma de claves que se transfiere a un equipo que no está ejecutando el sistema operativo Windows y, a continuación, se reemplaza o se combina con el archivo. *ad/etc/Krb5.keytab*existente. |
| /princ`<principalname>` | Especifica el nombre de la entidad de seguridad en el formulario host/computer.contoso.com@CONTOSO.COM . **ADVERTENCIA:** Este parámetro distingue entre mayúsculas y minúsculas. |
| /mapuser`<useraccount>` | Asigna el nombre de la entidad de seguridad de Kerberos, que se especifica mediante el parámetro **princ** , a la cuenta de dominio especificada. |
| /mapop`{add|set}` | Especifica cómo se establece el atributo de asignación.<ul><li>**Agregar** : agrega el valor del nombre de usuario local especificado. Este es el valor predeterminado.</li><li>**Set** : establece el valor para el cifrado solo del estándar de cifrado de datos (des) para el nombre de usuario local especificado.</li></ul> |
| `{-|+}`solo para | De forma predeterminada, se establece el cifrado de solo DES.<ul><li>**+** Establece una cuenta para el cifrado de solo DES.</li><li>**-** Libera la restricción en una cuenta para el cifrado solo DES. **Importante:** Windows no es compatible con DES de forma predeterminada.</li></ul> |
| /in`<filename>` | Especifica el archivo. de la operación de escritura que se va a leer de un equipo host que no está ejecutando el sistema operativo Windows. |
| /Pass`{password|*|{-|+}rndpass}` | Especifica una contraseña para el nombre de usuario de la entidad de seguridad que se especifica mediante el parámetro **princ** . Use `*` para solicitar una contraseña. |
| /minpass | Establece la longitud mínima de la contraseña aleatoria en 15 caracteres. |
| /maxpass | Establece la longitud máxima de la contraseña aleatoria en 256 caracteres. |
| /crypto`{DES-CBC-CRC|DES-CBC-MD5|RC4-HMAC-NT|AES256-SHA1|AES128-SHA1|All}` | Especifica las claves que se generan en el archivo de la clave de claves:<ul><li>**Des-CBC-CRC** : se usa por compatibilidad.</li><li>**Des-CBC-MD5** : se ajusta más estrechamente a la implementación de MIT y se usa por compatibilidad.</li><li>**RC4-HMAC-NT** : emplea el cifrado de 128 bits.</li><li>**AES256-SHA1** : emplea el cifrado AES256-CTS-HMAC-SHA1-96.</li><li>   **AES128-SHA1** : emplea el cifrado AES128-CTS-HMAC-SHA1-96.</li><li>**All** : indica que se pueden usar todos los tipos de cifrado admitidos.</li></ul><p>**Nota:** Dado que la configuración predeterminada se basa en versiones MIT anteriores, siempre debe usar el `/crypto` parámetro. |
| /itercount | Especifica el recuento de iteraciones que se usa para el cifrado AES. El valor predeterminado omite **itercount** para el cifrado no AES y establece el cifrado aes en 4.096. |
| /ptype`{KRB5_NT_PRINCIPAL|KRB5_NT_SRV_INST|KRB5_NT_SRV_HST}` | Especifica el tipo de entidad de seguridad.<ul><li>**KRB5_NT_PRINCIPAL** : el tipo de entidad de seguridad general (recomendado).</li><li>**KRB5_NT_SRV_INST** : la instancia de servicio de usuario</li><li>  **KRB5_NT_SRV_HST** : la instancia de servicio de host</li></ul> |
| /kvno`<keyversionnum>` | Especifica el número de versión de la clave. El valor predeterminado es 1. |
| /Answer`{-|+}` | Establece el modo de respuesta en segundo plano:<ul><li>**-** Las respuestas restablecer contraseñas se solicitan automáticamente **sin**.</li><li>**+** Respuestas restablecer contraseñas se solicita automáticamente con **sí**.</li></ul> |
| /target | Establece el controlador de dominio que se va a usar. El valor predeterminado es para que se detecte el controlador de dominio, en función del nombre de la entidad de seguridad. Si el nombre del controlador de dominio no se resuelve, un cuadro de diálogo solicitará un controlador de dominio válido. |
| /rawsalt | obliga a ktpass a usar el algoritmo rawsalt al generar la clave. Este parámetro es opcional. |
| `{-|+}dumpsalt` | La salida de este parámetro muestra el algoritmo de sal de MIT que se usa para generar la clave. |
| `{-|+}setupn` | Establece el nombre principal de usuario (UPN) además del nombre de entidad de seguridad de servicio (SPN). El valor predeterminado es establecer ambos en el archivo. |
| `{-|+}setpass <password>` | Establece la contraseña del usuario cuando se proporciona. Si se usa rndpass, en su lugar se genera una contraseña aleatoria. |
| /? | Muestra ayuda para este comando. |

#### <a name="remarks"></a>Observaciones

- Los servicios que se ejecutan en sistemas que no ejecutan el sistema operativo Windows se pueden configurar con cuentas de instancia de servicio en AD DS. Esto permite a cualquier cliente Kerberos autenticarse en servicios que no ejecutan el sistema operativo Windows mediante el uso de los KDC de Windows.

- Ktpass no evalúa el parámetro **/princ** y se usa tal y como se proporciona. No hay ninguna comprobación para ver si el parámetro coincide con las mayúsculas y minúsculas exactas del valor del atributo **userPrincipalName** al generar el archivo de la marca de claves. Las distribuciones de Kerberos que distinguen mayúsculas de minúsculas con este archivo de la misma pueden tener problemas si no hay ninguna coincidencia exacta entre mayúsculas y minúsculas, e incluso podrían producir errores durante la autenticación previa. Para comprobar y recuperar el valor de atributo **userPrincipalName** correcto de un archivo de exportación LDifDE. Por ejemplo:

    ```
    ldifde /f keytab_user.ldf /d CN=Keytab User,OU=UserAccounts,DC=contoso,DC=corp,DC=microsoft,DC=com /p base /l samaccountname,userprincipalname
    ````

### <a name="examples"></a>Ejemplos

Para crear un archivo Kerberos. de claves de claves para un equipo host que no ejecute el sistema operativo Windows, debe asignar la entidad de seguridad a la cuenta y establecer la contraseña de la entidad de seguridad del host.

1. Use el complemento **usuarios y equipos** de Active Directory para crear una cuenta de usuario para un servicio en un equipo que no esté ejecutando el sistema operativo Windows. Por ejemplo, cree una cuenta con el nombre *user1*.

2. Use el comando **ktpass** para configurar una asignación de identidad para la cuenta de usuario; para ello, escriba:

    ```
    ktpass /princ host/User1.contoso.com@CONTOSO.COM /mapuser User1 /pass MyPas$w0rd /out machine.keytab /crypto all /ptype KRB5_NT_PRINCIPAL /mapop set
    ```

    > [!NOTE]
    > No se pueden asignar varias instancias de servicio a la misma cuenta de usuario.

3. Combine el archivo. separador de claves con el archivo */etc/Krb5.keytab* en un equipo host que no esté ejecutando el sistema operativo Windows.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

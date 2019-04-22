---
title: Exportar una configuración de NPS para la importación en otro servidor
description: Puede utilizar este tema para obtener información sobre cómo exportar una configuración de servidor de directivas de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d268dc57-78f8-47ba-9a7a-a607e8b9225c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a5f28c317f1d58fd1889fb55d345463dc8a62999
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816426"
---
# <a name="export-an-nps-configuration-for-import-on-another-server"></a>Exportar una configuración de NPS para la importación en otro servidor

Se aplica a: Windows Server 2016

Puede exportar toda la configuración de NPS, incluidos clientes RADIUS y servidores, directivas de redes, directiva de solicitud de conexión, registro y configuración de registro: desde un servidor NPS para la importación en otro NPS. 

Para exportar la configuración de NPS, use una de las siguientes herramientas:

- En Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012, puede usar Netsh o puede usar Windows PowerShell.
- En Windows Server 2008 R2 y Windows Server 2008, use Netsh.

>[!IMPORTANT]
>No utilice este procedimiento si la base de datos NPS de origen tiene un número de versión mayor que el número de versión de la base de datos NPS de destino. Puede ver el número de versión de la base de datos NPS de la presentación de la **netsh nps muestran config** comando.

Dado que las configuraciones de NPS no están cifradas en el archivo XML exportado, enviarlo a través de una red podría suponer un riesgo de seguridad, por lo que tome precauciones al mover el archivo XML desde el servidor de origen a los servidores de destino. Por ejemplo, agregar el archivo a un archivo de almacenamiento protegido de contraseña cifrada, antes de pasar el archivo. Además, almacene el archivo en una ubicación segura para evitar que usuarios malintencionados accedan a él.

>[!NOTE]
>Si el registro de SQL Server está configurado en el origen de NPS, configuración del registro de SQL Server no se exporta al archivo XML. Después de importar el archivo en otro NPS, debe configurar manualmente el registro de SQL Server.

## <a name="export-and-import-the-nps-configuration-by-using-windows-powershell"></a>Exportar e importar la configuración de NPS mediante Windows PowerShell

Para Windows Server 2012 y versiones posteriores del sistema operativo, puede exportar la configuración de NPS mediante Windows PowerShell.

La sintaxis del comando para exportar la configuración de NPS es como sigue. 

    Export-NpsConfiguration -Path <filename>

En la tabla siguiente se enumera los parámetros para el **exportación NpsConfiguration** cmdlet de Windows PowerShell. Parámetros en negrita son obligatorios.

|Parámetro|Descripción|
|---------|-----------|
|Ruta de acceso|Especifica el nombre y la ubicación del archivo XML al que desea exportar la configuración de NPS.|

**Credenciales administrativas**

Para completar este procedimiento, debe ser miembro del grupo Administradores.

### <a name="export-example"></a>Ejemplo de exportación 

En el ejemplo siguiente, se exporta la configuración de NPS en un archivo XML que se encuentra en la unidad local. Para ejecutar este comando, ejecute Windows PowerShell como administrador en el origen de NPS, escriba el siguiente comando y presione ENTRAR.

`Export-NpsConfiguration –Path c:\config.xml` 

Para obtener más información, consulte [exportación NpsConfiguration](https://technet.microsoft.com/library/jj872749.aspx).

Después de haber exportado la configuración de NPS, copie el archivo XML en el servidor de destino.

La sintaxis del comando para importar la configuración de NPS en el servidor de destino es el siguiente.

    Import-NpsConfiguration [-Path] <String> [ <CommonParameters>]

### <a name="import-example"></a>Ejemplo de importación

El siguiente comando importa la configuración desde el archivo denominado C:\Npsconfig.xml a NPS. Para ejecutar este comando, ejecute Windows PowerShell como administrador en el NPS de destino, escriba el siguiente comando y presione ENTRAR.

    PS C:\> Import-NpsConfiguration -Path "C:\Npsconfig.xml"

Para obtener más información, consulte [importación NpsConfiguration](https://technet.microsoft.com/library/jj872750.aspx).

## <a name="export-and-import-the-nps-configuration-by-using-netsh"></a>Exportar e importar la configuración de NPS mediante el uso de Netsh

Puede usar el Shell de red \(Netsh\) para exportar la configuración de NPS mediante el **exportación netsh nps** comando.

Cuando el **importación netsh nps** se ejecuta el comando, NPS se actualiza automáticamente con la configuración actualizada. No es necesario detener el NPS en el equipo de destino para ejecutar el **importación netsh nps** comando, pero si la consola NPS o el complemento MMC NPS está abierta durante la importación de configuración, los cambios realizados en la configuración del servidor no son visibles hasta que Actualice la vista. 

>[!NOTE]
>Cuando se usa el **exportación netsh nps** comando, debe proporcionar el parámetro de comando **exportPSK** con el valor **Sí**. Este parámetro y valor indicar explícitamente que comprende que va a exportar la configuración de NPS, y que contiene el archivo XML exportado sin cifrar secretos compartidos para los clientes RADIUS y miembros de grupos de servidores RADIUS remotos.

**Credenciales administrativas**

Para completar este procedimiento, debe ser miembro del grupo Administradores.

### <a name="to-copy-an-nps-configuration-to-another-nps-using-netsh-commands"></a>Para copiar una configuración de NPS en otro NPS mediante comandos de Netsh

1. En el origen de NPS, abra **símbolo**, tipo **netsh**, y, a continuación, presione ENTRAR.

2. En el **netsh** escriba **nps**, y, a continuación, presione ENTRAR. 

3. En el **netsh nps** escriba **exportar filename =**"*path\file.xml*" **exportPSK = YES**, donde *derutadeacceso* es la ubicación de la carpeta donde desea guardar el archivo de configuración de NPS y *archivo* es el nombre del archivo XML que desea guardar. Presiona Entrar. 

Esto almacena valores de configuración \(, incluida la configuración del registro\) en un archivo XML. La ruta de acceso puede ser absoluta o relativa, o puede ser una convención de nomenclatura Universal \(UNC\) ruta de acceso. Después de presionar ENTRAR, aparece un mensaje que indica si la exportación a archivo ha sido correcta.

4. Copie el archivo que creó en el NPS de destino.

5. En el símbolo del sistema en el NPS de destino, escriba **netsh nps importar filename =**"*path\file.xml*", y, a continuación, presione ENTRAR. Aparece un mensaje indicando si la importación del archivo XML ha sido correcta.

Para obtener más información acerca de netsh, vea [Shell de red (Netsh)](../netsh/netsh.md).


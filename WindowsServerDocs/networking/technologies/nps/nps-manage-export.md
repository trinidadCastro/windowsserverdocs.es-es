---
title: Exportar una configuración del servidor NPS para importar en otro servidor
description: Puedes usar este tema para obtener información sobre cómo exportar una configuración del servidor de directivas de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d268dc57-78f8-47ba-9a7a-a607e8b9225c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 141a99e930672d8403315cb6804290d184ef3007
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="export-an-nps-server-configuration-for-import-on-another-server"></a>Exportar una configuración del servidor NPS para importar en otro servidor

Se aplica a: Windows Server 2016

Puedes exportar toda la configuración de NPS, incluidos los clientes de RADIUS y servidores, directiva de red, la directiva de solicitud de conexión, del registro y configuración de registro, desde un servidor NPS para importar en otro servidor NPS. 

Usa una de las siguientes herramientas para exportar la configuración de NPS:

- En Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012, puedes usar Netsh o puedes usar Windows PowerShell.
- En Windows Server 2008 R2 y Windows Server 2008, usa Netsh.

>[!IMPORTANT]
>No se usa este procedimiento si la base de datos NPS de origen tiene un número de versión superior al número de versión de la base de datos NPS de destino. Puedes ver el número de versión de la base de NPS la visualización de la **netsh nps Mostrar configuración** comando.

Dado que las configuraciones del servidor NPS no están cifradas en el archivo XML, enviar a través de una red puede suponer un riesgo de seguridad, por lo tanto, tomar precauciones al mover el archivo XML desde el servidor de origen para los servidores de destino. Por ejemplo, agregar el archivo a un archivo de almacenamiento protegido de contraseña cifrada, antes de pasar el archivo. Además, almacenar el archivo en una ubicación segura para impedir que los usuarios malintencionados obtener acceso a él.

>[!NOTE]
>Si se configura el registro de SQL Server en el servidor NPS de origen, la configuración de registro de SQL Server no se exporta al archivo XML. Después de importar el archivo en otro servidor NPS, debe configurar manualmente el registro de SQL Server.

## <a name="export-and-import-the-nps-configuration-by-using-windows-powershell"></a>Exportar e importar la configuración de NPS mediante Windows PowerShell

Para Windows Server 2012 y versiones posteriores del sistema operativo, puedes exportar la configuración de NPS mediante Windows PowerShell.

La sintaxis de comandos para exportar la configuración de NPS es la siguiente. 

    Export-NpsConfiguration -Path <filename>

La siguiente tabla enumera los parámetros para el **exportación NpsConfiguration** cmdlet de Windows PowerShell. Se requieren parámetros en negrita.

|Parámetro|Descripción|
|---------|-----------|
|Ruta de acceso|Especifica el nombre y la ubicación del archivo XML a la que quieres exportar la configuración del servidor NPS.|

**Credenciales administrativas**

Para completar este procedimiento, debe ser miembro del grupo de administradores.

### <a name="export-example"></a>Ejemplo de exportación 

En el ejemplo siguiente, se exporta la configuración de NPS a un archivo XML que se encuentra en la unidad local. Para ejecutar este comando, ejecutar Windows PowerShell como administrador en el servidor NPS de origen, escribe el siguiente comando y presione ENTRAR.

`Export-NpsConfiguration –Path c:\config.xml` 

Para obtener más información, consulta [exportación NpsConfiguration](https://technet.microsoft.com/library/jj872749.aspx).

Después de haber exportado la configuración de NPS, copia el archivo XML en el servidor de destino.

La sintaxis del comando para importar la configuración de NPS en el servidor de destino es la siguiente.

    Import-NpsConfiguration [-Path] <String> [ <CommonParameters>]

### <a name="import-example"></a>Ejemplo de importación

El siguiente comando importa la configuración del archivo denominado C:\Npsconfig.xml a NPS. Para ejecutar este comando, ejecutar Windows PowerShell como administrador en el servidor NPS de destino, escribe el siguiente comando y presione ENTRAR.

    PS C:\> Import-NpsConfiguration -Path "C:\Npsconfig.xml"

Para obtener más información, consulta [importar NpsConfiguration](https://technet.microsoft.com/library/jj872750.aspx).

## <a name="export-and-import-the-nps-configuration-by-using-netsh"></a>Exportar e importar la configuración de NPS mediante Netsh

Puedes usar el Shell de red \(Netsh\) para exportar la configuración del servidor NPS mediante la **netsh nps exportar** comando.

Cuando la **netsh nps importar** se ejecuta el comando, NPS se actualizará automáticamente con la configuración actualizada. No es necesario detener NPS en el equipo de destino para ejecutar el **netsh nps importar** comando, pero si la consola NPS o el complemento MMC NPS está abierto durante la importación de configuración, cambios en la configuración del servidor no son visibles hasta que actualices la vista. 

>[!NOTE]
>Cuando usas el **netsh nps exportar** comando, es necesario para proporcionar el parámetro de comando **exportPSK** con el valor **Sí**. Este parámetro y valor indicar explícitamente que comprenden que va a exportar la configuración del servidor NPS y que contiene el archivo XML sin cifrar secretos compartidos para clientes de RADIUS y los miembros de grupos de servidores RADIUS remotos.

**Credenciales administrativas**

Para completar este procedimiento, debe ser miembro del grupo de administradores.

### <a name="to-copy-an-nps-server-configuration-to-another-nps-server-using-netsh-commands"></a>Para copiar una configuración del servidor NPS a otro servidor NPS que usar los comandos Netsh

1. En el servidor NPS de origen, abra **símbolo**, tipo **netsh**, y, a continuación, presione ENTRAR.

2. En la **netsh**, escriba **nps**, y, a continuación, presione ENTRAR. 

3. En la **netsh nps**, escriba **exportar el nombre de archivo =**"*path\file.xml*" **exportPSK es "yes"**, donde *ruta de acceso* es la ubicación de carpeta donde quieres guardar el servidor NPS archivo de configuración, y *archivo* es el nombre del archivo XML que desea guardar. Presiona ENTRAR. 

Almacena valores de configuración \(including registry settings\) en un archivo XML. La ruta de acceso puede ser relativo o absoluto, o puede ser una ruta de acceso de convención de nomenclatura Universal \(UNC\). Después de presionar ENTRAR, aparece un mensaje que indica si la exportación al archivo ha terminado correctamente.

4. Copia el archivo creado en el servidor NPS de destino.

5. En el símbolo del sistema en el servidor NPS de destino, escriba **netsh nps importar el nombre de archivo =**"*path\file.xml*", y, a continuación, presione ENTRAR. Aparecerá un mensaje que indica si la importación del archivo XML se realizó correctamente.

Para obtener más información acerca de netsh, consulta [Shell de red (Netsh)](../netsh/netsh.md).


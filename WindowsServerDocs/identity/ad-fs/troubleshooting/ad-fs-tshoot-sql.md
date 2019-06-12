---
title: Solución de AD FS - conectividad de SQL
description: Este documento describe cómo solucionar diversos aspectos de AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4b09094b6e305bc85b38e94d11fbc8845d555437
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443933"
---
# <a name="ad-fs-troubleshooting---sql-connectivity"></a>Solución de AD FS - conectividad de SQL
AD FS proporciona la capacidad para usar el servidor SQL remoto para los datos de la granja de servidores de AD FS.  Verá problemas si los servidores de AD FS en la granja de servidores no pueden comunicarse con los servidores back-end SQL Server.  El siguiente documento proporciona algunos pasos básicos para probar la comunicación con los servidores back-end.

## <a name="acquire-the-sql-database-connection-string"></a>Adquirir la cadena de conexión de base de datos SQL
Es lo primero que se prueba al comprobar la conectividad de SQL, si AD FS tiene la información de conexión SQL correcta.  Esto puede hacerse mediante PowerShell.

### <a name="to-acquire-the-sql-connection-string"></a>Para obtener la cadena de conexión SQL
1.  Abra Windows PowerShell
2. Escriba lo siguiente: `$adfs = gwmi -Namespace root/ADFS -Class SecurityTokenService` y presione ENTRAR
3. Escriba lo siguiente: `$adfs.ConfigurationDatabaseConnectionString` y presione ENTRAR.
4. Debería ver la información de la cadena de conexión.
![](media/ad-fs-tshoot-sql/sql2.png)

## <a name="create-a-universal-data-link-udl-file-to-test-connectivity"></a>Cree un archivo de vínculo de datos Universal (UDL) para probar la conectividad
Un archivo UDL o el archivo UDL es básicamente un archivo de texto que contiene la cadena de conexión de base de datos.  Podemos probar si el servidor SQL server está respondiendo a las conexiones con la información que hemos obtenidos anteriormente.

### <a name="to-create-a-udl-file-to-test-connectivity"></a>Para crear un archivo udl para probar la conectividad

1. Abra el Bloc de notas y guarde el archivo como test.udl.  Asegúrese de que tiene **todos los archivos** seleccionado en la lista desplegable para **Guardar como tipo**.
2. Haga doble clic en test.udl
3. Rellene la información siguiente: una. **Seleccione o escriba un nombre de servidor:**  Utilice el origen de datos de la cadena de conexión anterior b. **Especificar información para iniciar sesión en el servidor:**  Usar la cuenta de servicio de AD FS o una cuenta que tenga permisos de inicio de sesión remoto.  Si la cuenta es un uso de la cuenta de windows integrado autenticación en caso contrario, escriba el nombre de usuario y la contraseña.
    c. **Seleccione la base de datos en el servidor:** Usar el catálogo inicial de la cadena anterior.  Por ejemplo:  AdfsConfigurationV3.
   ![Probar conexión](media/ad-fs-tshoot-sql/sql4.png)
1. Haga clic en **Probar conexión**.</br>
![Éxito](media/ad-fs-tshoot-sql/sql3.png)

## <a name="use-sql-server-management-studio-to-test-connectivity"></a>Usar SQL Server Management Studio para probar la conectividad
También puede [descargar](https://go.microsoft.com/fwlink/?linkid=864329) e instalación de SSMS para probar la conectividad de base de datos.

### <a name="to-test-connectivity-with-ssms"></a>Para probar la conectividad con SSMS
1. Descargue e instale SQL Server Management Studio.
![Instalar](media/ad-fs-tshoot-sql/sql5.png)
1. Abra SSMS, escriba el nombre del servidor.  El origen de datos anterior.
2. Usar la cuenta de servicio de AD FS o una cuenta que tenga permisos de inicio de sesión remoto.  Si la cuenta es un uso de la cuenta de windows integrado autenticación en caso contrario, escriba el nombre de usuario y la contraseña.
![Conectar](media/ad-fs-tshoot-sql/sql6.png)
1. Debería ver el lado izquierdo que se rellena.  Expanda bases de datos y compruebe que ve las bases de datos de AD FS.
![Bases de datos de AD FS](media/ad-fs-tshoot-sql/sql7.png)

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)
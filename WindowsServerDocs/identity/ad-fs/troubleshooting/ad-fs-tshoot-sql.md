---
title: Solución de problemas de AD FS-conectividad de SQL
description: Obtenga información acerca de cómo solucionar diversos aspectos de AD FS como se aplica a la conectividad de SQL.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2017
ms.topic: article
ms.openlocfilehash: 0d68e71c4ac4d1e3d03bd3c549ccc6f82e68ebe5
ms.sourcegitcommit: 605a9b46b74b2c7a9116e631e902467ea02a6e70
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2021
ms.locfileid: "97965807"
---
# <a name="ad-fs-troubleshooting---sql-connectivity"></a>Solución de problemas de AD FS-conectividad de SQL
AD FS proporciona la capacidad de usar SQL Server remotos para los datos de la granja de AD FS.  Verá problemas si los servidores de AD FS de la granja no pueden comunicarse con los servidores SQL Server de back-end.  En el siguiente documento se proporcionarán algunos pasos básicos para probar la comunicación con los servidores back-end.

## <a name="acquire-the-sql-database-connection-string"></a>Adquisición de la cadena de conexión de SQL Database
Lo primero que se debe probar al comprobar la conectividad de SQL es si AD FS tiene la información de conexión SQL correcta.  Esto se puede hacer mediante PowerShell.

### <a name="to-acquire-the-sql-connection-string"></a>Para adquirir la cadena de conexión SQL
1.  Abrir Windows PowerShell
2. Escriba lo siguiente: `$adfs = gwmi -Namespace root/ADFS -Class SecurityTokenService` y presione Entrar.
3. Escriba lo siguiente:  `$adfs.ConfigurationDatabaseConnectionString` y presione Entrar.
4. Debería ver la información de la cadena de conexión.

![Pantalla de comandos de PowerShell en ejecución (comando)](media/ad-fs-tshoot-sql/sql2.png)

## <a name="create-a-universal-data-link-udl-file-to-test-connectivity"></a>Crear un archivo de vínculo de datos universal (UDL) para probar la conectividad
Un archivo de vínculo de datos universal o un archivo UDL es básicamente un archivo de texto que contiene una cadena de conexión de base de datos.  Mediante el uso de la información que se obtuvo anteriormente, se puede probar si el servidor SQL Server responde a las conexiones.

### <a name="to-create-a-testudl-file-to-test-connectivity"></a>Para crear un archivo test. udl para probar la conectividad

1. Abra el Bloc de notas y guarde el archivo como test. udl.  Asegúrese de que tiene **todos los archivos** seleccionados en el menú desplegable para **Guardar como tipo**.
2. Haga doble clic en test. udl
3. Rellene la información siguiente: a. **Seleccione o escriba un nombre de servidor:**  Use el origen de datos de la cadena de conexión anterior a b. **Escriba la información para iniciar sesión en el servidor:**  Use la cuenta de servicio de AD FS o una cuenta que tenga permisos para iniciar sesión de forma remota.  Si la cuenta es una cuenta de Windows, use la autenticación integrada; de lo contrario, escriba el nombre de usuario y la contraseña.
    c. **Seleccione la base de datos en el servidor:** Use el catálogo inicial de la cadena anterior.  Ejemplo: AdfsConfigurationV3.
   ![Probar conexión](media/ad-fs-tshoot-sql/sql4.png)
1. Haga clic en **Probar conexión**.</br>
![Success](media/ad-fs-tshoot-sql/sql3.png)

## <a name="use-sql-server-management-studio-to-test-connectivity"></a>Usar SQL Server Management Studio para probar la conectividad
También puede [Descargar](https://go.microsoft.com/fwlink/?linkid=864329) e instalar SSMS para probar la conectividad de la base de datos.

### <a name="to-test-connectivity-with-ssms"></a>Para probar la conectividad con SSMS
1. Descargue e instale SQL Server Management Studio.
![Instalación](media/ad-fs-tshoot-sql/sql5.png)
1. Abra SSMS y escriba el nombre del servidor.  Origen de datos anterior.
2. Use la cuenta de servicio de AD FS o una cuenta que tenga permisos para iniciar sesión de forma remota.  Si la cuenta es una cuenta de Windows, use la autenticación integrada; de lo contrario, escriba el nombre de usuario y la contraseña.
![Conexión](media/ad-fs-tshoot-sql/sql6.png)
1. Debería ver el lado izquierdo rellenado.  Expanda bases de datos y compruebe que ve las bases de datos de AD FS.
![Bases de datos de AD FS](media/ad-fs-tshoot-sql/sql7.png)

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)
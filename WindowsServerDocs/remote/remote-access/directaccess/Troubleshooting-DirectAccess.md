---
title: Solución de problemas de DirectAccess
description: En este tema se proporciona información sobre solución de problemas de las implementaciones de DirectAccess en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 61040e19-5960-4eb0-b612-d710627988f7
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ec725eea286c359461b0f4a7b8763b97464e7067
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867096"
---
# <a name="troubleshooting-directaccess"></a>Solución de problemas de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Siga estos pasos para solucionar problemas de acceso remoto (DirectAccess).  
  
|||  
|-|-|  
|**Problema**|**Resolución**|  
|Consola de administración de acceso remoto no puede mostrar la configuración de DirectAccess|**Para restaurar la información de configuración que falta**<br />-Si está solucionando una implementación multisitio, asegúrese de que el controlador de dominio más cercano al punto de entrada está disponible.<br />-Use el **Get-DAEntrypointDC** cmdlet para recuperar el nombre del controlador de dominio más cercano al punto de entrada. Si no se está ejecutando el controlador de dominio, use el **Set-DAEntryPointDC** cmdlet para que apunte a otro controlador de dominio.<br />-Ejecute **gpresult** desde un símbolo del sistema con privilegios elevados en el servidor para asegurarse de que el servidor recibe los objetos de directiva de grupo de DirectAccess.<br />-Habilitar el registro de la interfaz (IU) de usuario.<br />-Use el siguiente comando para iniciar el registro de Windows PowerShell:<pre>logman create trace ETWTrace -ow -o c:\ETWTrace.etl -p {AAD4C46D-56DE-4F98-BDA2-B5EAEBDD2B04} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode 0x2 -max 2048 -ets <br />logman update trace ETWTrace -p {62DFF3DA-7513-4FCA-BC73-25B111FBB1DB} 0xffffffffffffffff 0xff -ets</pre><repro>-Cerrar y volver a abrir la interfaz de usuario.<br />-Deshabilitar el registro de Windows Powershell. Recopilar los archivos de registro de seguimiento de eventos. Además, recopilar todos los registros de la **%windir%/tracing** carpeta.|  
|Aplicar la configuración de DirectAccess se produce un error|**Para actualizar la configuración de DirectAccess**<br />-Si está solucionando una implementación multisitio, asegúrese de que el controlador de dominio más cercano al punto de entrada está disponible.<br />-Use el **Get-DAEntrypointDC** cmdlet para recuperar el nombre del controlador de dominio más cercano al punto de entrada. Si no se está ejecutando el controlador de dominio, use el **Set-DAEntryPointDC** cmdlet para que apunte a otro controlador de dominio.<br />-Use el siguiente comando para iniciar el registro de Windows Powershell:<br /><pre>logman create trace ETWTrace -ow -o c:\ETWTrace.etl -p {AAD4C46D-56DE-4F98-BDA2-B5EAEBDD2B04} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode 0x2 -max 2048 -ets<br />logman update trace ETWTrace -p {62DFF3DA-7513-4FCA-BC73-25B111FBB1DB} 0xffffffffffffffff 0xff -ets</pre>    <repro><br />-Haga clic en **aplicar**.<br />-Después de que se produce el error, deshabilite el registro de Windows Powershell y recopilar el registro de seguimiento de eventos.|  
|Configurar DirectAccess, pero los clientes no son capaz de conectarse a los recursos internos|**Para solucionar problemas de conexión de cliente**<br />-Haga clic en el **estado de las operaciones** pestaña en la consola de administración de acceso remoto y asegúrese de que todos los componentes muestran un icono verde. Si no es así, compruebe los detalles del error y siga los pasos de resolución.<br />-Ejecute acceso remoto Server Best Practices Analyzer (BPA). Si hay advertencias o errores, siga los pasos de resolución para resolver el problema.|  
|Encontrar problemas relacionados con una configuración multisitio (por ejemplo, habilitar los puntos de entrada multisitio, agregar o establecimiento del controlador de dominio para un punto de entrada)|Siga los pasos de [solucionar problemas de una implementación multisitio](https://technet.microsoft.com/library/jj554657(v=ws.11).aspx).|  
|Icono de estado de configuración en el panel muestra una advertencia o error|Siga los pasos de [supervisar el estado de distribución de la configuración del servidor de acceso remoto](https://technet.microsoft.com/library/jj574221(v=ws.11).aspx).|  
|Encontrar problemas relacionados con la configuración (por ejemplo, la configuración produce un error al habilitar Equilibrio de carga, o hay problemas al agregar o quitar servidores de un clúster) de equilibrio de carga|Si se han habilitar Equilibrio de carga o agregar un nodo y la configuración se actualiza al hacer clic **aplicar**, pero el clúster no forma correctamente en el servidor, ejecute el siguiente comando: **cmd.exe /c "reg add HKLM\ SYSTEM\CurrentControlSet\Services\RaMgmtSvc\Parameters /f /v DebugFlag /t REG_DWORD /d "" 0xffffffff"" "** para el usuario de recopilar registros de interfaz en el nuevo servidor.|  
|Estado de las operaciones muestra un error o advertencia después de los pasos siguientes para corregir la situación|Si el estado de las operaciones se muestra información incorrecta (por ejemplo, errores, incluso después corregirlos):<br /><br />-Habilitar la clave del registro **cmd.exe /c "reg add HKLM\SYSTEM\CurrentControlSet\Services\RaMgmtSvc\Parameters /f /v EnableTracing /t REG_DWORD /d""5" ""**.<br />-Actualizar el estado de las operaciones y recopile los registros en **%windir%/tracing**.|  
|Windows 8 y versiones posteriores equipos cliente de DirectAccess de informe "N Internet" como estado de la conexión de DirectAccess y conectividad limitada de informes de indicador de estado de conectividad de red (NCSI).|Esto puede ocurrir cuando el túnel forzado está habilitado en la configuración de DirectAccess y, por este motivo, se usa solo para IPHTTPS. Para resolver este problema, puede crear y configurar un servidor proxy. NCSI, a continuación, utiliza el servidor proxy para realizar comprobaciones de conectividad de Internet. Se recomienda agregar a un proxy estático de a la tabla de directivas de resolución de nombres (NRPT) mediante el procedimiento siguiente.<br /><br />Antes de ejecutar los comandos en este procedimiento, asegúrese de reemplazar todos los nombres de dominio, los nombres de equipo y otras variables de comando de Windows PowerShell con los valores adecuados para su implementación.<br /><br />**Configurar a un proxy estático para una regla de NRPT**<br />1.  Para mostrar el "." Regla de NRPT: `Get-DnsClientNrptRule -GpoName "corp.example.com\DirectAccess Client Settings" -Server <DomainControllerNetBIOSName>`<br />2.  Tenga en cuenta el nombre (GUID) de la "." Regla de NRPT. El nombre (GUID) debe comenzar con **DA-{.}**<br />3.  Establecer el proxy para el "." Regla de NRPT para **proxy.corp.example.com:8080**:  `Set-DnsClientNrptRule -Name "DA-{..}" -Server <DomainControllerNetBIOSName> -GPOName "corp.example.com\DirectAccess Client Settings" -DAProxyServerName "proxy.corp.example.com:8080" -DAProxyType "UseProxyName"`<br />4.  Para mostrar el "." Regla de NRPT nuevo mediante la ejecución de `Get-DnsClientNrptRule`y compruebe que **ProxyFQDN:port** ahora está configurado correctamente.<br />5.  Actualice la directiva de grupo ejecutando `gpupdate /force` en un cliente de DirectAccess cuando el cliente se conecta internamente, a continuación, se muestran la NRPT con `Get-DnsClientNrptPolicy` y compruebe que el "." regla muestra **ProxyFQDN:port**.|  
  



---
title: Detección, habilitación y deshabilitación de SMBv1, SMBv2 y SMBv3 en Windows
description: Describe cómo habilitar y deshabilitar el protocolo de bloque de mensajes del servidor (SMBv1, SMBv2 y SMBv3) en entornos de cliente y servidor de Windows.
author: Deland-Han
manager: dcscontentpm
audience: ITPro
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 9da4d6f2b5616dc6f8aec3fefb1ae7141ed88b0b
ms.sourcegitcommit: 8cf04db0bc44fd98f4321dca334e38c6573fae6c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/03/2020
ms.locfileid: "75654396"
---
# <a name="how-to-detect-enable-and-disable-smbv1-smbv2-and-smbv3-in-windows"></a>Detección, habilitación y deshabilitación de SMBv1, SMBv2 y SMBv3 en Windows

## <a name="summary"></a>Resumen

En este artículo se describe cómo habilitar y deshabilitar el bloque de mensajes del servidor (SMB) versión 1 (SMBv1), SMB versión 2 (SMBv2) y SMB versión 3 (SMBv3) en los componentes de servidor y cliente SMB. 

> [!IMPORTANT]
> Se recomienda **no** deshabilitar SMBv2 o SMBv3. Deshabilite SMBv2 o SMBv3 solo como medida de solución de problemas temporal. No deje la deshabilitación de SMBv2 o SMBv3.  

En Windows 7 y Windows Server 2008 R2, al deshabilitar SMBv2 se desactiva la siguiente funcionalidad: 
 
- Solicitar composición: permite el envío de varias solicitudes de SMB 2 como una única solicitud de red    
- Lecturas y escrituras más grandes: uso mejorado de redes más rápidas    
- Almacenamiento en caché de propiedades de archivos y carpetas: los clientes mantienen copias locales de carpetas y archivos    
- Manipuladores duraderos: permitir que la conexión se vuelva a conectar de forma transparente al servidor si se produce una desconexión temporal    
- Firma de mensajes mejorada: HMAC SHA-256 reemplaza MD5 como algoritmo hash    
- Escalabilidad mejorada para el uso compartido de archivos: número de usuarios, recursos compartidos y archivos abiertos por servidor que han aumentado considerablemente    
- Compatibilidad con vínculos simbólicos    
- Modelo de concesión de bloqueo oportunista de cliente: limita los datos transferidos entre el cliente y el servidor, mejorando el rendimiento en redes de alta latencia y aumentando la escalabilidad del servidor SMB.    
- Compatibilidad con MTU grande: para el uso completo de Ethernet de 10 GIGABYE (GB)    
- Eficiencia energética mejorada: los clientes que tienen archivos abiertos en un servidor pueden entrar en suspensión    

En Windows 8, Windows 8.1, Windows 10, Windows Server 2012 y Windows Server 2016, al deshabilitar SMBv3 se desactiva la siguiente funcionalidad (y también la funcionalidad de SMBv2 que se describe en la lista anterior): 
 
- Conmutación por error transparente: los clientes se vuelven a conectar sin interrupción a los nodos del clúster durante el mantenimiento o la conmutación por error    
- Escalabilidad horizontal: acceso simultáneo a los datos compartidos en todos los nodos de clúster de archivos     
- Multicanal: agregación de ancho de banda de red y tolerancia a errores si hay varias rutas de acceso disponibles entre el cliente y el servidor  
- SMB directo: agrega compatibilidad con redes RDMA para un rendimiento muy alto, con baja latencia y uso de CPU bajo    
- Cifrado: proporciona cifrado de un extremo a otro y evita la interceptación en redes que no son de confianza.    
- Concesión de directorios: mejora los tiempos de respuesta de las aplicaciones en las sucursales a través del almacenamiento en caché    
- Optimizaciones de rendimiento: optimizaciones para e/s de lectura y escritura aleatorias pequeñas

##  <a name="more-information"></a>Más información

El protocolo SMBv2 se presentó en Windows Vista y Windows Server 2008.

El protocolo SMBv3 se presentó en Windows 8 y Windows Server 2012.

Para obtener más información sobre las funcionalidades de las capacidades de SMBv2 y SMBv3, consulte los siguientes artículos:

[Información general de bloque de mensajes del servidor](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831795(v=ws.11))

[Novedades de SMB](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff625695(v=ws.10))  

## <a name="how-to-gracefully-remove-smb-v1-in-windows-81-windows-10-windows-2012-r2-and-windows-server-2016"></a>Cómo quitar correctamente SMB V1 en Windows 8.1, Windows 10, Windows 2012 R2 y Windows Server 2016

#### <a name="windows-server-2012-r2--2016-powershell-methods"></a>Windows Server 2012 R2 & 2016: métodos de PowerShell

##### <a name="smb-v1"></a>SMB v1

- Determinar 

  ```PowerShell
  Get-WindowsFeature FS-SMB1
  ```

- Activa 

  ```PowerShell
  Disable-WindowsOptionalFeature -Online -FeatureName smb1protocol
  ```

- Habilite: 

  ```PowerShell
  Enable-WindowsOptionalFeature -Online -FeatureName smb1protocol
  ```

##### <a name="smb-v2v3"></a>SMB V2/V3

- Determinar
  
  ```PowerShell
  Get-SmbServerConfiguration | Select EnableSMB2Protocol
  ```

- Activa

  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB2Protocol $false
  ```

- Habilite:

  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB2Protocol $true 
  ```

#### <a name="windows-server-2012-r2-and-windows-server-2016-server-manager-method-for-disabling-smb"></a>Windows Server 2012 R2 y Windows Server 2016: método Administrador del servidor para deshabilitar SMB

##### <a name="smb-v1"></a>SMB v1

![Método Administrador del servidor-Dashboard](media/detect-enable-and-disable-smbv1-v2-v3-1.png)

#### <a name="windows-81-and-windows-10-powershell-method"></a>Windows 8.1 y Windows 10: método de PowerShell

##### <a name="smb-v1-protocol"></a>Protocolo SMB v1

- Determinar 
  
  ```PowerShell
  Get-WindowsOptionalFeature –Online –FeatureName SMB1Protocol
  ```

- Activa 

  ```PowerShell
  Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
  ```

- Habilite: 

  ```PowerShell
  Enable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
  ```

##### <a name="smb-v2v3protocol"></a>Protocolo SMB V2/V3

- Determinar 
  
  ```PowerShell
  Get-SmbServerConfiguration | Select EnableSMB2Protocol
  ```

- Activa 
  
  ```PowerShell
  Set-SmbServerConfiguration –EnableSMB2Protocol $false
  ```

- Habilite:

  ```PowerShell
  Set-SmbServerConfiguration –EnableSMB2Protocol $true
  ```

#### <a name="windows-81-and-windows-10-add-or-remove-programs-method"></a>Windows 8.1 y Windows 10: método para agregar o quitar programas

![Método de cliente agregar o quitar programas](media/detect-enable-and-disable-smbv1-v2-v3-2.png)

## <a name="how-to-detect-status-enable-and-disable-smb-protocols-on-the-smb-server"></a>Cómo detectar el estado, habilitar y deshabilitar los protocolos SMB en el servidor SMB

### <a name="for-windows-8-and-windows-server-2012"></a>Para Windows 8 y Windows Server 2012

Windows 8 y Windows Server 2012 presentan el nuevo cmdlet **set-SMBServerConfiguration de** Windows PowerShell. El cmdlet permite habilitar o deshabilitar los protocolos SMBv1, SMBv2 y SMBv3 en el componente del servidor.  

> [!NOTE]   
> Al habilitar o deshabilitar SMBv2 en Windows 8 o Windows Server 2012, SMBv3 también está habilitado o deshabilitado. Este comportamiento se produce porque estos protocolos comparten la misma pila.     

No es necesario reiniciar el equipo después de ejecutar el cmdlet **set-SMBServerConfiguration** . 

##### <a name="smb-v1-on-smb-server"></a>SMB V1 en el servidor SMB

- Determinar 
  
  ```PowerShell
  Get-SmbServerConfiguration | Select EnableSMB1Protocol
  ```

- Activa 

  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB1Protocol $false
  ```

- Habilite: 
  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB1Protocol $true
  ```

Para obtener más información, consulte [almacenamiento de servidor en Microsoft](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/Stop-using-SMB1/ba-p/425858). 
##### <a name="smb-v2v3-on-smb-server"></a>SMB V2/V3 en el servidor SMB

- Determinar

  ```PowerShell
  Get-SmbServerConfiguration | Select EnableSMB2Protocol
  ```

- Activa 
  
  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB2Protocol $false
  ```

- Habilite:
  
  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB2Protocol $true
  ```

### <a name="for-windows-7-windows-server-2008-r2-windows-vista-and-windows-server-2008"></a>Para Windows 7, Windows Server 2008 R2, Windows Vista y Windows Server 2008

Para habilitar o deshabilitar los protocolos SMB en un servidor SMB que sea con Windows 7, Windows Server 2008 R2, Windows Vista o Windows Server 2008, use Windows PowerShell o el editor del registro. 

#### <a name="powershell-methods"></a>Métodos de PowerShell

> [!NOTE]
> Este método requiere PowerShell 2,0 o una versión posterior de PowerShell.

##### <a name="smb-v1-on-smb-server"></a>SMB V1 en el servidor SMB

Determinar

```PowerShell
Get-Item HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters | ForEach-Object {Get-ItemProperty $_.pspath}
```

Configuración predeterminada = habilitada (no se crea ninguna clave de registro), por lo que no se devolverá ningún valor de SMB1

Activa

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB1 -Type DWORD -Value 0 –Force
```

Habilite:  

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB1 -Type DWORD -Value 1 –Force
```  

**Nota:** Debe reiniciar el equipo después de realizar estos cambios. Para obtener más información, consulte [almacenamiento de servidor en Microsoft](https://blogs.technet.microsoft.com/filecab/2016/09/16/stop-using-smb1/). 
##### <a name="smb-v2v3-on-smb-server"></a>SMB V2/V3 en el servidor SMB

Determinar  

```PowerShell
Get-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters | ForEach-Object {Get-ItemProperty $_.pspath} 
```

Activa

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB2 -Type DWORD -Value 0 –Force  
```

Habilite:

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB2 -Type DWORD -Value 1 –Force 
```

> [!NOTE]
> Debe reiniciar el equipo después de realizar estos cambios. 

#### <a name="registry-editor"></a>Editor del registro

> [!IMPORTANT]
> Sigue meticulosamente los pasos que se describen en esta sección. Pueden producirse problemas graves si modificas el registro de manera incorrecta. Antes de modificarlo, [haz una copia de seguridad del registro para restaurarlo](https://support.microsoft.com/help/322756), por si se produjeran problemas.
 
Para habilitar o deshabilitar SMBv1 en el servidor SMB, configure la siguiente clave del registro:

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters**

```
Registry entry: SMB1
REG_DWORD: 0 = Disabled
REG_DWORD: 1 = Enabled
Default: 1 = Enabled (No registry key is created)
```

Para habilitar o deshabilitar SMBv2 en el servidor SMB, configure la siguiente clave del registro: 

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters**

```
Registry entry: SMB2
REG_DWORD: 0 = Disabled
REG_DWORD: 1 = Enabled
Default: 1 = Enabled (No registry key is created) 
```

> [!NOTE]
> debe reiniciar el equipo después de realizar estos cambios. 

## <a name="how-to-detect-status-enable-and-disable-smb-protocols-on-the-smb-client"></a>Detección de estado, habilitación y deshabilitación de protocolos SMB en el cliente SMB

### <a name="for-windows-vista-windows-server-2008-windows-7-windows-server-2008-r2-windows-8-and-windows-server-2012"></a>Para Windows Vista, Windows Server 2008, Windows 7, Windows Server 2008 R2, Windows 8 y Windows Server 2012

> [!NOTE]
> Al habilitar o deshabilitar SMBv2 en Windows 8 o en Windows Server 2012, SMBv3 también está habilitado o deshabilitado. Este comportamiento se produce porque estos protocolos comparten la misma pila. 

##### <a name="smb-v1-on-smb-client"></a>SMB V1 en el cliente SMB

- Detectar
  
  ```cmd
  sc.exe qc lanmanworkstation
  ```

- Activa
  
  ```cmd
  sc.exe config lanmanworkstation depend= bowser/mrxsmb20/nsi
  sc.exe config mrxsmb10 start= disabled
  ```

- Habilite:

  ```cmd
  sc.exe config lanmanworkstation depend= bowser/mrxsmb10/mrxsmb20/nsi
  sc.exe config mrxsmb10 start= auto
  ```

Para obtener más información, consulte [almacenamiento de servidor en Microsoft](https://blogs.technet.microsoft.com/filecab/2016/09/16/stop-using-smb1/) . 
##### <a name="smb-v2v3-on-smb-client"></a>SMB V2/V3 en el cliente SMB

- Determinar

  ```cmd
  sc.exe qc lanmanworkstation
  ```

- Activa

  ```cmd
  sc.exe config lanmanworkstation depend= bowser/mrxsmb10/nsi
  sc.exe config mrxsmb20 start= disabled 
  ```

- Habilite:

  ```cmd
  sc.exe config lanmanworkstation depend= bowser/mrxsmb10/mrxsmb20/nsi
  sc.exe config mrxsmb20 start= auto
  ```

> [!NOTE]
> - Debe ejecutar estos comandos en un símbolo del sistema con privilegios elevados.    
> - Debe reiniciar el equipo después de realizar estos cambios.    
 

## <a name="disable-smbv1-server-with-group-policy"></a>Deshabilitar SMBv1 Server con directiva de grupo

En este procedimiento se configura el siguiente elemento nuevo en el registro:

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters** 

- Entrada del registro: **SMB1** 
- REG_DWORD: **0** = deshabilitado   

Para configurarlo mediante directiva de grupo, siga estos pasos:
 
1. Abra la **Consola de administración de directivas de grupo**. Haz clic con el botón derecho en el objeto de directiva de grupo (GPO) que debería contener el nuevo elemento de preferencia y luego haz clic en **Editar**.

2. En el árbol de consola, en **configuración del equipo**, expanda la carpeta **preferencias** y, a continuación, expanda la carpeta **configuración de Windows** .

3. Haga clic con el botón secundario en el nodo **registro** , elija **nuevo**y seleccione **elemento del registro**.

   ![Registro: nuevo elemento del registro](media/detect-enable-and-disable-smbv1-v2-v3-3.png)    
 
En el cuadro de diálogo **propiedades del nuevo registro**, seleccione lo siguiente: 
 
- **Acción**: crear    
- **Hive**: HKEY_LOCAL_MACHINE    
- **Ruta de acceso**de la clave: SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters    
- **Nombre del valor**: SMB1    
- **Tipo de valor**: REG_DWORD    
- **Datos del valor**: 0    
 
![Nuevas propiedades del registro: General](media/detect-enable-and-disable-smbv1-v2-v3-4.png)

Esto deshabilita los componentes de servidor de SMBv1. Esta directiva de grupo debe aplicarse a todas las estaciones de trabajo, servidores y controladores de dominio necesarios en el dominio.

> [!NOTE]
>Los  [filtros WMI](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc947846(v=ws.10)) también se pueden configurar para excluir sistemas operativos no compatibles o exclusiones seleccionadas, como Windows XP.

> [!IMPORTANT]
> Tenga cuidado al realizar estos cambios en los controladores de dominio en los que Windows XP o los sistemas de terceros y de terceros heredados (que no admiten SMBv2 o SMBv3) requieran acceso a SYSVOL u otros recursos compartidos de archivos donde se deshabilite SMB v1.     

## <a name="disable-smbv1-client-with-group-policy"></a>Deshabilitar el cliente de SMBv1 con directiva de grupo

Para deshabilitar el cliente de SMBv1, es necesario actualizar la clave del registro de servicios para deshabilitar el inicio de **MRxSMB10** y, a continuación, la dependencia de **MRxSMB10** debe quitarse de la entrada de **LanmanWorkstation** , de modo que pueda iniciarse normalmente sin requerir **MRxSMB10** para iniciarse por primera vez.

Esto actualizará y reemplazará los valores predeterminados en los dos elementos siguientes del registro: 

**HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\services\mrxsmb10** 

Entrada del registro: **inicio** REG_DWORD: **4**= deshabilitado

**HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\LanmanWorkstation** 

Entrada del registro: **DependOnService** REG_MULTI_SZ: **"examinar", "MRxSmb20", "NSI"**   

> [!NOTE]
> el valor predeterminado incluido MRxSMB10, que ahora se quita como dependencia.

Para configurarlo mediante directiva de grupo, siga estos pasos:
 
1. Abra la **Consola de administración de directivas de grupo**. Haz clic con el botón derecho en el objeto de directiva de grupo (GPO) que debería contener el nuevo elemento de preferencia y luego haz clic en **Editar**.

2. En el árbol de consola, en **configuración del equipo**, expanda la carpeta **preferencias** y, a continuación, expanda la carpeta **configuración de Windows** .

3. Haga clic con el botón secundario en el nodo **registro** , elija **nuevo**y seleccione **elemento del registro**.    

4. En el cuadro de diálogo **propiedades del nuevo registro** , seleccione lo siguiente: 
 
   - **Acción**: actualizar
   - **Hive**: HKEY_LOCAL_MACHINE
   - **Ruta de acceso**de la clave: SYSTEM\CurrentControlSet\services\mrxsmb10
   - **Nombre del valor**: Start
   - **Tipo de valor**: REG_DWORD
   - **Datos del valor**: 4
 
   ![Propiedades de inicio-general](media/detect-enable-and-disable-smbv1-v2-v3-5.png)

5. A continuación, quite la dependencia en el **MRxSMB10** que se acaba de deshabilitar.

   En el cuadro de diálogo **propiedades del nuevo registro** , seleccione lo siguiente: 
 
   - **Acción**: reemplazar
   - **Hive**: HKEY_LOCAL_MACHINE
   - **Ruta de acceso**de la clave: SYSTEM\CurrentControlSet\Services\LanmanWorkstation
   - **Nombre del valor**: DependOnService
   - **Tipo de valor**: REG_MULTI_SZ 
   - **Información del valor**:
      - Examinar
      - MRxSmb20
      - NSI
 
   > [!NOTE]
   > Estas tres cadenas no tendrán viñetas (vea la siguiente captura de pantalla).

   ![Propiedades de DependOnService](media/detect-enable-and-disable-smbv1-v2-v3-6.png) 

   El valor predeterminado incluye **MRxSMB10** en muchas versiones de Windows, por lo que al reemplazarlos por esta cadena de varios valores, está en vigor al quitar **MRxSMB10** como una dependencia para **LanManServer** y pasar de cuatro valores predeterminados hasta solo estos tres valores anteriores.

   > [!NOTE]
   > Cuando se usa Consola de administración de directivas de grupo, no es necesario usar comillas o comas. Basta con escribir cada entrada en líneas individuales.

6. Reinicie los sistemas de destino para finalizar la deshabilitación de SMB v1.

### <a name="summary"></a>Resumen

Si todos los valores están en el mismo directiva de grupo objeto (GPO), directiva de grupo administración muestra la siguiente configuración.

![Editor de administración de directivas de grupo-registro](media/detect-enable-and-disable-smbv1-v2-v3-7.png) 

### <a name="testing-and-validation"></a>Pruebas y validación

Una vez configurados, permita que la Directiva se replique y actualice. Según sea necesario para las pruebas, ejecute **gpupdate/force** en un símbolo del sistema y, a continuación, revise los equipos de destino para asegurarse de que la configuración del registro se aplica correctamente. Asegúrese de que SMB V2 y SMB V3 funcionen para todos los demás sistemas del entorno.

> [!NOTE]
> No olvide reiniciar los sistemas de destino.

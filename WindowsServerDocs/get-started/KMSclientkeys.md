---
title: Claves de configuración de cliente KMS
description: Claves necesarias para activar los productos de Windows desde un servidor KMS
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: jaimeo
ms.localizationpriority: medium
ms.date: 10/02/2018
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.openlocfilehash: 57ce4c4d7623c2a424efbdf0ff117ede8fad726b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847036"
---
# <a name="kms-client-setup-keys"></a>Claves de configuración del cliente KMS

>Se aplica a: Windows Server 2019, canal semianual de Windows Server, Windows Server 2016, Windows 10

Los ordenadores que ejecutan ediciones con licencias por volumen de Windows Server, Windows 10, Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7, Windows Server 2008 R2, Windows Vista y Windows Server 2008 son, de forma predeterminada, clientes KMS que no necesitan configuración adicional.

>[!NOTE]
> En las tablas siguientes, "LTSC" es el acrónimo "Canal de mantenimiento a largo plazo," mientras que "LTSB" hace referencia a la "a largo plazo rama de mantenimiento". 

**Para usar las claves que aquí (que son Gvlk), primero debe tener un host de KMS que se ejecuta en su implementación.** Si aún no ha configurado un host de KMS, consulte [Deploy KMS Activation](https://technet.microsoft.com/library/dn502531(v=ws.11).aspx) para ver los pasos para configurar uno.

Si convierte un equipo de un host de KMS, MAK o edición comercial de Windows a un cliente de KMS, instale la clave de configuración correspondiente (GVLK) que se incluye en las tablas siguientes. Para instalar una clave de configuración de cliente, abra un símbolo del sistema administrativo en el cliente, escriba **slmgr /ipk \<clave de configuración\>**  y, a continuación, presione **ENTRAR**.

| Si desea...                                                                                                                                                                                          | ...use estos recursos                                                                                                         |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| Activa Windows fuera de un escenario de activación por volumen (es decir, intentar activar una versión comercial de Windows), en cuyo caso **estas claves no funcionarán**.                                                     | Use estos vínculos para versiones comerciales de Windows:                                                                              |
| Corregir este error aparece cuando intenta activar un Windows 8.1, Windows Server 2012 R2 o un sistema más reciente: "Error: 0xC004F050 El Servicio de licencias de software notificó que la clave de producto no es válida"… | [Instale esta actualización](https://support.microsoft.com/en-us/help/3172614/july-2016-update-rollup-for-windows-8-1-and-windows-server-2012-r2) en el host de KMS si está ejecutando Windows 8.1, Windows Server 2012 R2, Windows 8 o Windows Server 2012. |

-   [Obtenga Windows 10](https://www.microsoft.com/en-us/windows/get-windows-10)

-   [Obtener una nueva clave de producto de Windows](https://support.microsoft.com/help/10749/windows-product-key)

-   [Ayuda de Windows original y procedimientos](https://support.microsoft.com/help/15087/windows-genuine)


>   Si estás ejecutando Windows Server 2008 R2 o Windows 7, estate atento por si hay una actualización que admita su uso como hosts de KMS para clientes de Windows 10.


## <a name="windows-server-semi-annual-channel-versions"></a>Versiones de canal semianual de Windows Server

### <a name="windows-server-version-1809"></a>Windows Server, versión 1809
| Edición de sistema operativo       | Clave de configuración de cliente de KMS          |
|--------------------------------|-------------------------------|
| Windows Server Datacenter | 6NMRW-2C8FM-D24W7-TQWMY-CWH2D  | 
| Windows Server Standard | N2KJX-J94YW-TQVFB-DG9YT-724CC  |


### <a name="windows-server-version-1803"></a>Windows Server, versión 1803

| Edición de sistema operativo       | Clave de configuración de cliente de KMS          |
|--------------------------------|-------------------------------|
| Windows Server Datacenter | 2HXDN-KRXHB-GPYC7-YCKFJ-7FVDG  | 
| Windows Server Standard   | PTXN8-JFHJM-4WC78-MPCBR-9W4KR  |

### <a name="windows-server-version-1709"></a>Windows Server, versión 1709

| Edición de sistema operativo       | Clave de configuración de cliente de KMS          |
|--------------------------------|-------------------------------|
| Windows Server Datacenter | 6Y6KB-N82V8-D8CQV-23MJW-BWTG6  | 
| Windows Server Standard   | DPCNP-XQFKJ-BJF7R-FRC8D-GF6G4  |

## <a name="windows-server-ltscltsb-versions"></a>Versiones de LTSC/LTSB de Windows Server

### <a name="windows-server-2019"></a>Windows Server 2019
| Edición de sistema operativo       | Clave de configuración de cliente de KMS          |
|--------------------------------|-------------------------------|
| Windows Server 2019 Datacenter | WMDGN-G9PQG-XVVXX-R3X43-63DFG  | 
| Windows Server 2019 Standard   | N69G4-B89J2-4G8F4-WWYCC-J464C  |
| Windows Server 2019 Essentials|WVDHN-86M7X-466P6-VHXV7-YY726|

### <a name="windows-server-2016"></a>Windows Server 2016

| Edición de sistema operativo       | Clave de configuración de cliente de KMS          |
|--------------------------------|-------------------------------|
| Windows Server 2016 Datacenter | CB7KF-BWN84-R7R2Y-793K2-8XDDG |
| Windows Server 2016 Standard   | WC2BQ-8NRM3-FDDYY-2BFGV-KHKQY |
| Windows Server 2016 Essentials | JCKRF-N37P4-C2D82-9YXRT-4M63B |

## <a name="windows-10-all-supported-semi-annual-channel-versions"></a>Windows 10, todas las versiones de canal semianual compatibles

Consulte la [hoja de información del ciclo de vida de Windows](https://support.microsoft.com/en-us/help/13853/windows-lifecycle-fact-sheet) para obtener información acerca de las versiones compatibles y el final de las fechas de servicio.

| Edición de sistema operativo          | Clave de configuración de cliente de KMS          |
|-----------------------------------|-------------------------------|
|Windows 10 Pro|W269N-WFGWX-YVC9B-4J6C9-T83GX|
|Windows 10 Pro N|MH37W-N47XK-V7XM9-C7227-GCQG9|
|Estaciones de trabajo de Windows 10 Pro|NRG8B-VKK3Q-CXVCJ-9G2XF-6Q84J|
|Estaciones de trabajo de Windows 10 Pro N|9FNHH-K3HBT-3W4TD-6383H-6XYWF|
|Windows 10 Pro Education|6TP4R-GNPTD-KYYHQ-7B7DP-J447Y|
|Windows 10 Pro Education N|YVWGF-BXNMC-HTQYQ-CPQ99-66QFC|
|Windows 10 Education|NW6C2-QMPVW-D7KKK-3GKT6-VCFB2|
|Windows 10 Education N |2WH4N-8QGBV-H22JP-CT43Q-MDWWJ|
|Windows 10 Enterprise  |NPPR9-FWDCX-D2C8J-H872K-2YT43|
|Windows 10 Enterprise N    |DPH2V-TTNVB-4X9Q3-TJR4H-KHJW4|
|Windows 10 Enterprise G|YYVX9-NTFWV-6MDM3-9PT4T-4M68B|
|Windows 10 Enterprise G N|44RPN-FTY23-9VTTB-MP9BX-T84FV|

## <a name="windows-10-ltscltsb-versions"></a>Versiones de Windows 10 LTSC/LTSB

### <a name="windows-10-ltsc-2019"></a>Windows 10 LTSC 2019

|Edición de sistema operativo|Clave de configuración de cliente de KMS|
|-|-|
|Windows 10 Enterprise LTSC 2019|M7XTQ-FN8P6-TTKYV-9D4CC-J462D|
|Windows 10 Enterprise N LTSC 2019|92NFX-8DJQP-P6BBQ-THF9C-7CG2H|

### <a name="windows-10-ltsb-2016"></a>Windows 10 LTSB 2016

|Edición de sistema operativo|Clave de configuración de cliente de KMS|
|-|-|
|Windows 10 Enterprise LTSB 2016|DCPHK-NFMTC-H88MJ-PFHPY-QJ4BJ|
|Windows 10 Enterprise N LTSB 2016|QFFDN-GRT3P-VKWWX-X7T3R-8B639|

### <a name="windows-10-ltsb-2015"></a>Windows 10 LTSB 2015 

| Edición de sistema operativo          | Clave de configuración de cliente de KMS          |
|-----------------------------------|-------------------------------|
| Windows 10 Enterprise 2015 LTSB   | WNMTR-4C88C-JK8YV-HQ7T2-76DF9 |
| Windows 10 Enterprise 2015 LTSB N | 2F77B-TNFGY-69QQF-B8YKP-D69TJ |

## <a name="earlier-versions-of-windows-server"></a>Versiones anteriores de Windows Server
### <a name="windows-server-2012-r2"></a>Windows Server 2012 R2

| Edición de sistema operativo               | Clave de configuración de cliente de KMS          |
|----------------------------------------|-------------------------------|
| Windows Server 2012 R2 Server Standard | D2N9P-3P6X9-2R39C-7RTCD-MDVJX |
| Windows Server 2012 R2 Datacenter      | W3GGN-FT8W3-Y4M27-J84CP-Q3VJ9 |
| Windows Server 2012 R2 Essentials      | KNC87-3J2TX-XB4WP-VCPJV-M4FWM |

### <a name="windows-server-2012"></a>Windows Server 2012

| Edición de sistema operativo                | Clave de configuración de cliente de KMS          |
|-----------------------------------------|-------------------------------|
| Windows Server 2012                     | BN3D2-R7TKB-3YPBD-8DRP2-27GG4 |
| Windows Server 2012 N                   | 8N2M2-HWPGY-7PGT9-HGDD8-GVGGY |
| Windows Server 2012 Single Language     | 2WN2H-YGCQR-KFX6K-CD6TF-84YXQ |
| Windows Server 2012 Country Specific    | 4K36P-JN4VD-GDC6V-KDT89-DYFKP |
| Windows Server 2012 Server Standard     | XC9B7-NBPP2-83J2H-RHMBY-92BT4 |
| Windows Server 2012 MultiPoint Standard | HM7DN-YVMH3-46JC3-XYTG7-CYQJJ |
| Windows Server 2012 MultiPoint Premium  | XNH6W-2V9GX-RGJ4K-Y8X6F-QGJ2G |
| Windows Server 2012 Datacenter          | 48HP8-DN98B-MYWDG-T2DCC-8W83P |


### <a name="windows-server-2008-r2"></a>Windows Server 2008 R2

| Edición de sistema operativo                         | Clave de configuración de cliente de KMS          |
|--------------------------------------------------|-------------------------------|
| Windows Server 2008 R2 Web                       | 6TPJF-RBVHG-WBW2R-86QPH-6RTM4 |
| Windows Server 2008 R2 HPC Edition               | TT8MH-CG224-D3D7Q-498W2-9QCTX |
| Windows Server 2008 R2 Standard                  | YC6KT-GKW9T-YTKYR-T4X34-R7VHC |
| Windows Server 2008 R2 Enterprise                | 489J6-VHDMP-X63PK-3K798-CPX3Y |
| Windows Server 2008 R2 Datacenter                | 74YFP-3QFB3-KQT8W-PMXWJ-7M648 |
| Windows Server 2008 R2 for Itanium-based Systems | GT63C-RJFQ3-4GMB6-BRFB9-CB83V |

### <a name="windows-server-2008"></a>Windows Server 2008

| Edición de sistema operativo                       | Clave de configuración de cliente de KMS          |
|------------------------------------------------|-------------------------------|
| Windows Web Server 2008                        | WYR28-R7TFJ-3X2YQ-YCY4H-M249D |
| Windows Server 2008 Standard                   | TM24T-X9RMF-VWXK6-X8JC9-BFGM2 |
| Windows Server 2008 Standard sin Hyper-V   | W7VD6-7JFBR-RX26B-YKQ3Y-6FFFJ |
| Windows Server 2008 Enterprise                 | YQGMW-MPWTJ-34KDK-48M3W-X4Q6V |
| Windows Server 2008 Enterprise sin Hyper-V | 39BXF-X8Q23-P2WWT-38T2F-G3FPG |
| Windows Server 2008 HPC                        | RCTX3-KWVHP-BR6TB-RB6DM-6X7HP |
| Windows Server 2008 Datacenter                 | 7M67G-PC374-GR742-YH8V4-TCBY3 |
| Windows Server 2008 Datacenter sin Hyper-V | 22XQ2-VRXRG-P8D42-K34TD-G3QQC |
| Windows Server 2008 for Itanium-Based Systems  | 4DWFP-JF3DJ-B7DTH-78FJB-PDRHK |

## <a name="earlier-versions-of-windows"></a>Versiones anteriores de Windows

### <a name="windows-81"></a>Windows 8.1

| Edición de sistema operativo               | Clave de configuración de cliente de KMS          |
|----------------------------------------|-------------------------------|
| Windows 8.1 Pro               | GCRJD-8NW9H-F2CDX-CCM8D-9D6T9 |
| Windows 8.1 Pro N             | HMCNV-VVBFX-7HMBH-CTY9B-B4FXY |
| Windows 8.1 Enterprise                 | MHF9N-XY6XB-WVXMC-BTDCT-MKKG7 |
| Windows 8.1 Enterprise N               | TT4HM-HN7YT-62K67-RGRQJ-JFFXW |

### <a name="windows-8"></a>Windows 8

| Edición de sistema operativo                | Clave de configuración de cliente de KMS          |
|-----------------------------------------|-------------------------------|
| Windows 8 Pro                  | NG4HW-VH26C-733KW-K6F98-J8CK4 |
| Windows 8 Pro N                | XCVCF-2NXM9-723PB-MHCB7-2RYQQ |
| Windows 8 Enterprise                    | 32JNW-9KQ84-P47T8-D8GGY-CWCK7 |
| Windows 8 Enterprise N                  | JMNMF-RHW7P-DMY6X-RF3DR-X2BQT |


### <a name="windows-7"></a>Windows 7 

| Edición de sistema operativo                         | Clave de configuración de cliente de KMS          |
|--------------------------------------------------|-------------------------------|
| Windows 7 Professional                           | FJ82H-XT6CR-J8D7P-XQJJ2-GPDD4 |
| Windows 7 Professional N                         | MRPKT-YTG23-K7D7T-X2JMM-QY7MG |
| Windows 7 Professional E                         | W82YF-2Q76Y-63HXB-FGJG9-GF7QX |
| Windows 7 Enterprise                             | 33PXH-7Y6KF-2VJC9-XBBR8-HVTHH |
| Windows 7 Enterprise N                           | YDRBP-3D83W-TY26F-D46B2-XCKRJ |
| Windows 7 Enterprise E                           | C29WB-22CC8-VJ326-GHFJW-H9DH4 |


Vea también

• [Planificar la activación por volumen](https://technet.microsoft.com/library/jj134042(v=ws.11).aspx)



---
title: Comandos de Windows
description: Referencia
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c703d07c-8227-4e86-94a6-8ef390f94cdc
author: jasongerend
ms.author: jgerend
manager: dongill
ms.date: 06/09/2020
ms.prod: windows-server
ms.openlocfilehash: 66de80652f764840af70e88dfd39df2398ae495c
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721122"
---
# <a name="windows-commands"></a>Comandos de Windows

Todas las versiones compatibles de Windows (servidor y cliente) tienen un conjunto de comandos de consola Win32 integrados.

En este conjunto de documentación se describen los comandos de Windows que se pueden usar para automatizar tareas mediante scripts o herramientas de scripting.

Para obtener información acerca de un comando específico, en el siguiente menú A-Z, haga clic en la letra con la que comienza el comando y, a continuación, haga clic en el nombre del comando.

[Un](#a)  |
 [B](#b)  |
 [C](#c)  |
 [D](#d)  |
 [E](#e)  |
 [F](#f)  |
 [G](#g)  |
 [H](#h)  |
 [I](#i)  |
 [J](#j)  |
 [K](#k)  |
 [L](#l)  |
 [M](#m)  |
 [N](#n)  |
 [O](#o)  |
 [P](#p)  |
 [P](#q)  |
 [R](#r)  |
 [S](#s)  |
 [T](#t)  |
 [U](#u)  |
 [V](#v)  |
 [W](#w)  |
 [X](#x) | S | Z

## <a name="prerequisites"></a>Requisitos previos

La información contenida en este tema se aplica a:

- Windows Server 2019
- Windows Server (Canal semianual)
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2
- Windows Server 2008
- Windows 10
- Windows 8.1

### <a name="command-shell-overview"></a>Información general del shell de comandos

El shell de comandos era el primer Shell integrado en Windows para automatizar las tareas rutinarias, como la administración de cuentas de usuario o las copias de seguridad nocturnas, con archivos de Batch (. bat). Con Windows Script Host, podía ejecutar scripts más sofisticados en el shell de comandos. Para obtener más información, vea [cscript](cscript.md) o [Wscript](wscript.md). Puede realizar operaciones de forma más eficaz mediante el uso de scripts que puede usar la interfaz de usuario. Los scripts aceptan todos los comandos que están disponibles en la línea de comandos.

Windows tiene dos shells de comandos: el shell de comandos y [PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6). Cada Shell es un programa de software que proporciona comunicación directa entre usted y el sistema operativo o la aplicación, lo que proporciona un entorno para automatizar las operaciones de ti.

PowerShell se diseñó para ampliar las funcionalidades del shell de comandos para ejecutar comandos de PowerShell denominados cmdlets. Los cmdlets son similares a los comandos de Windows, pero proporcionan un lenguaje de scripting extensible. Puede ejecutar comandos de Windows y cmdlets de PowerShell en PowerShell, pero el shell de comandos solo puede ejecutar comandos de Windows y no cmdlets de PowerShell.

Para la automatización de Windows más sólida y actualizada, se recomienda usar PowerShell en lugar de comandos de Windows o Windows Script Host para la automatización de Windows.

> [!NOTE]
>También puede descargar e instalar [PowerShell Core](https://docs.microsoft.com/powershell/scripting/whats-new/what-s-new-in-powershell-core-60?view=powershell-6), la versión de código abierto de PowerShell.

> [!CAUTION]
> La edición incorrecta del Registro puede dañar gravemente el sistema. Antes de realizar los siguientes cambios en el registro, debe realizar una copia de seguridad de los datos valiosos del equipo.

> [!NOTE]
> Para habilitar o deshabilitar la finalización de nombres de archivos y directorios en el shell de comandos de un equipo o una sesión de inicio de sesión de usuario, ejecute **regedit.exe** y establezca el siguiente **valor de reg_DWOrd**:
>
> HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\completionChar\ reg_DWOrd
>
> Para establecer el valor de **reg_DWOrd** , use el valor hexadecimal de un carácter de control para una función determinada (por ejemplo, **0 9** es Tab y **0 08** es el retroceso). La configuración especificada por el usuario tiene prioridad sobre la configuración del equipo y las opciones de línea de comandos tienen prioridad sobre la configuración del registro.

## <a name="command-line-reference-a-z"></a>Referencia A-Z de la línea de comandos  

Para obtener información acerca de un comando específico de Windows, en el siguiente menú A-Z, haga clic en la letra con la que comienza el comando y, a continuación, haga clic en el nombre del comando.

[Un](#a)  |
 [B](#b)  |
 [C](#c)  |
 [D](#d)  |
 [E](#e)  |
 [F](#f)  |
 [G](#g)  |
 [H](#h)  |
 [I](#i)  |
 [J](#j)  |
 [K](#k)  |
 [L](#l)  |
 [M](#m)  |
 [N](#n)  |
 [O](#o)  |
 [P](#p)  |
 [P](#q)  |
 [R](#r)  |
 [S](#s)  |
 [T](#t)  |
 [U](#u)  |
 [V](#v)  |
 [W](#w)  |
 [X](#x) | S | Z

### <a name="a"></a>A

- [active](active.md)
- [add](add.md)
- [add alias](add-alias.md)
- [add volume](add-volume.md)
- [append](append.md)
- [arp](arp.md)
- [assign](assign.md)
- [assoc](assoc.md)
- [at](at.md)
- [atmadm](atmadm.md)
- [attach-vdisk](attach-vdisk.md)
- [attrib](attrib.md)
- [attributes](attributes.md)
  - [attributes disk](attributes-disk.md)
  - [attributes volume](attributes-volume.md)
- [auditpol](auditpol.md)
  - [auditpol backup](auditpol-backup.md)
  - [auditpol clear](auditpol-clear.md)
  - [auditpol get](auditpol-get.md)
  - [auditpol list](auditpol-list.md)
  - [auditpol remove](auditpol-remove.md)
  - [auditpol resourcesacl](auditpol-resourcesacl.md)
  - [auditpol restore](auditpol-restore.md)
  - [auditpol set](auditpol-set.md)
- [autochk](autochk.md)
- [autoconv](autoconv.md)
- [autofmt](autofmt.md)
- [automount](automount.md)

### <a name="b"></a>B

- [bcdboot](bcdboot.md)
- [bcdedit](bcdedit.md)
- [bdehdcfg](bdehdcfg.md)
  - [bdehdcfg driveinfo](bdehdcfg-driveinfo.md)
  - [bdehdcfg newdriveletter](bdehdcfg-newdriveletter.md)
  - [bdehdcfg quiet](bdehdcfg-quiet.md)
  - [bdehdcfg restart](bdehdcfg-restart.md)
  - [bdehdcfg size](bdehdcfg-size.md)
  - [bdehdcfg target](bdehdcfg-target.md)
- [begin backup](begin-backup.md)
- [begin restore](begin-restore.md)
- [bitsadmin](bitsadmin.md)
  - [bitsadmin addfile](bitsadmin-addfile.md)
  - [bitsadmin addfileset](bitsadmin-addfileset.md)
  - [bitsadmin addfilewithranges](bitsadmin-addfilewithranges.md)
  - [bitsadmin cache](bitsadmin-cache.md)
    - [bitsadmin cache y delete](bitsadmin-cache-and-delete.md)
    - [bitsadmin cache y deleteurl](bitsadmin-cache-and-deleteurl.md)
    - [bitsadmin cache y getexpirationtime](bitsadmin-cache-and-getexpirationtime.md)
    - [bitsadmin cache y getlimit](bitsadmin-cache-and-getlimit.md)
    - [bitsadmin cache y help](bitsadmin-cache-and-help.md)
    - [bitsadmin cache e info](bitsadmin-cache-and-info.md)
    - [bitsadmin cache y list](bitsadmin-cache-and-list.md)
    - [bitsadmin cache y setexpirationtime](bitsadmin-cache-and-setexpirationtime.md)
    - [bitsadmin cache y setlimit](bitsadmin-cache-and-setlimit.md)
    - [bitsadmin cache y clear](bitsadmin-cache-clear.md)
  - [bitsadmin cancel](bitsadmin-cancel.md)
  - [bitsadmin complete](bitsadmin-complete.md)
  - [bitsadmin create](bitsadmin-create.md)
  - [Ejemplos de bitsadmin](bitsadmin-examples.md)
  - [bitsadmin getaclflags](bitsadmin-getaclflags.md)
  - [bitsadmin getbytestotal](bitsadmin-getbytestotal.md)
  - [bitsadmin getbytestransferred](bitsadmin-getbytestransferred.md)
  - [bitsadmin getclientcertificate](bitsadmin-getclientcertificate.md)
  - [bitsadmin getcompletiontime](bitsadmin-getcompletiontime.md)
  - [bitsadmin getcreationtime](bitsadmin-getcreationtime.md)
  - [bitsadmin getcustomheaders](bitsadmin-getcustomheaders.md)
  - [bitsadmin getdescription](bitsadmin-getdescription.md)
  - [bitsadmin getdisplayname](bitsadmin-getdisplayname.md)
  - [bitsadmin geterror](bitsadmin-geterror.md)
  - [bitsadmin geterrorcount](bitsadmin-geterrorcount.md)
  - [bitsadmin getfilestotal](bitsadmin-getfilestotal.md)
  - [bitsadmin getfilestransferred](bitsadmin-getfilestransferred.md)
  - [bitsadmin gethelpertokenflags](bitsadmin-gethelpertokenflags.md)
  - [bitsadmin gethelpertokensid](bitsadmin-gethelpertokensid.md)
  - [bitsadmin gethttpmethod](bitsadmin-gethttpmethod.md)
  - [bitsadmin getmaxdownloadtime](bitsadmin-getmaxdownloadtime.md)
  - [bitsadmin getminretrydelay](bitsadmin-getminretrydelay.md)
  - [bitsadmin getmodificationtime](bitsadmin-getmodificationtime.md)
  - [bitsadmin getnoprogresstimeout](bitsadmin-getnoprogresstimeout.md)
  - [bitsadmin getnotifycmdline](bitsadmin-getnotifycmdline.md)
  - [bitsadmin getnotifyflags](bitsadmin-getnotifyflags.md)
  - [bitsadmin getnotifyinterface](bitsadmin-getnotifyinterface.md)
  - [bitsadmin getowner](bitsadmin-getowner.md)
  - [bitsadmin getpeercachingflags](bitsadmin-getpeercachingflags.md)
  - [bitsadmin getpriority](bitsadmin-getpriority.md)
  - [bitsadmin getproxybypasslist](bitsadmin-getproxybypasslist.md)
  - [bitsadmin getproxylist](bitsadmin-getproxylist.md)
  - [bitsadmin getproxyusage](bitsadmin-getproxyusage.md)
  - [bitsadmin getreplydata](bitsadmin-getreplydata.md)
  - [bitsadmin getreplyfilename](bitsadmin-getreplyfilename.md)
  - [bitsadmin getreplyprogress](bitsadmin-getreplyprogress.md)
  - [bitsadmin getsecurityflags](bitsadmin-getsecurityflags.md)
  - [bitsadmin getstate](bitsadmin-getstate.md)
  - [bitsadmin gettemporaryname](bitsadmin-gettemporaryname.md)
  - [bitsadmin gettype](bitsadmin-gettype.md)
  - [bitsadmin getvalidationstate](bitsadmin-getvalidationstate.md)
  - [bitsadmin help](bitsadmin-help.md)
  - [bitsadmin info](bitsadmin-info.md)
  - [bitsadmin list](bitsadmin-list.md)
  - [bitsadmin listfiles](bitsadmin-listfiles.md)
  - [bitsadmin makecustomheaderswriteonly](bitsadmin-makecustomheaderswriteonly.md)
  - [bitsadmin monitor](bitsadmin-monitor.md)
  - [bitsadmin nowrap](bitsadmin-nowrap.md)
  - [bitsadmin peercaching](bitsadmin-peercaching.md)
    - [bitsadmin peercaching y getconfigurationflags](bitsadmin-peercaching-and-getconfigurationflags.md)
    - [bitsadmin peercaching y help](bitsadmin-peercaching-and-help.md)
    - [bitsadmin peercaching y setconfigurationflags](bitsadmin-peercaching-and-setconfigurationflags.md)
  - [bitsadmin peers](bitsadmin-peers.md)
    - [bitsadmin peers y clear](bitsadmin-peers-and-clear.md)
    - [bitsadmin peers y discover](bitsadmin-peers-and-discover.md)
    - [bitsadmin peers y help](bitsadmin-peers-and-help.md)
    - [bitsadmin peers y list](bitsadmin-peers-and-list.md)
  - [bitsadmin rawreturn](bitsadmin-rawreturn.md)
  - [bitsadmin removeclientcertificate](bitsadmin-removeclientcertificate.md)
  - [bitsadmin removecredentials](bitsadmin-removecredentials.md)
  - [bitsadmin replaceremoteprefix](bitsadmin-replaceremoteprefix.md)
  - [bitsadmin reset](bitsadmin-reset.md)
  - [bitsadmin resume](bitsadmin-resume.md)
  - [bitsadmin setaclflag](bitsadmin-setaclflag.md)
  - [bitsadmin setclientcertificatebyid](bitsadmin-setclientcertificatebyid.md)
  - [bitsadmin setclientcertificatebyname](bitsadmin-setclientcertificatebyname.md)
  - [bitsadmin setcredentials](bitsadmin-setcredentials.md)
  - [bitsadmin setcustomheaders](bitsadmin-setcustomheaders.md)
  - [bitsadmin setdescription](bitsadmin-setdescription.md)
  - [bitsadmin setdisplayname](bitsadmin-setdisplayname.md)
  - [bitsadmin sethelpertoken](bitsadmin-sethelpertoken.md)
  - [bitsadmin sethelpertokenflags](bitsadmin-sethelpertokenflags.md)
  - [bitsadmin sethttpmethod](bitsadmin-sethttpmethod.md)
  - [bitsadmin setmaxdownloadtime](bitsadmin-setmaxdownloadtime.md)
  - [bitsadmin setminretrydelay](bitsadmin-setminretrydelay.md)
  - [bitsadmin setnoprogresstimeout](bitsadmin-setnoprogresstimeout.md)
  - [bitsadmin setnotifycmdline](bitsadmin-setnotifycmdline.md)
  - [bitsadmin setnotifyflags](bitsadmin-setnotifyflags.md)
  - [bitsadmin setpeercachingflags](bitsadmin-setpeercachingflags.md)
  - [bitsadmin setpriority](bitsadmin-setpriority.md)
  - [bitsadmin setproxysettings](bitsadmin-setproxysettings.md)
  - [bitsadmin setreplyfilename](bitsadmin-setreplyfilename.md)
  - [bitsadmin setsecurityflags](bitsadmin-setsecurityflags.md)
  - [bitsadmin setvalidationstate](bitsadmin-setvalidationstate.md)
  - [bitsadmin suspend](bitsadmin-suspend.md)
  - [bitsadmin takeownership](bitsadmin-takeownership.md)
  - [bitsadmin transfer](bitsadmin-transfer.md)
  - [bitsadmin util](bitsadmin-util.md)
    - [bitsadmin util y enableanalyticchannel](bitsadmin-util-and-enableanalyticchannel.md)
    - [bitsadmin util y getieproxy](bitsadmin-util-and-getieproxy.md)
    - [bitsadmin util y help](bitsadmin-util-and-help.md)
    - [bitsadmin util y repairservice](bitsadmin-util-and-repairservice.md)
    - [bitsadmin util y setieproxy](bitsadmin-util-and-setieproxy.md)
    - [bitsadmin util y version](bitsadmin-util-and-version.md)
  - [bitsadmin wrap](bitsadmin-wrap.md)
- [bootcfg](bootcfg.md)
  - [bootcfg addsw](bootcfg-addsw.md)
  - [bootcfg copy](bootcfg-copy.md)
  - [bootcfg dbg1394](bootcfg-dbg1394.md)
  - [bootcfg debug](bootcfg-debug.md)
  - [bootcfg default](bootcfg-default.md)
  - [bootcfg delete](bootcfg-delete.md)
  - [bootcfg ems](bootcfg-ems.md)
  - [bootcfg query](bootcfg-query.md)
  - [bootcfg raw](bootcfg-raw.md)
  - [bootcfg rmsw](bootcfg-rmsw.md)
  - [bootcfg timeout](bootcfg-timeout.md)
- [break](break_1.md)

### <a name="c"></a>C

- [cacls](cacls.md)
- [call](call.md)
- [cd](cd.md)
- [certreq](certreq_1.md)
- [certutil](certutil.md)
- [change](change.md)
  - [change logon](change-logon.md)
  - [change port](change-port.md)
  - [change user](change-user.md)
- [chcp](chcp.md)
- [chdir](chdir.md)
- [chglogon](chglogon.md)
- [chgport](chgport.md)
- [chgusr](chgusr.md)
- [chkdsk](chkdsk.md)
- [chkntfs](chkntfs.md)
- [choice](choice.md)
- [cipher](cipher.md)
- [clean](clean.md)
- [cleanmgr](cleanmgr.md)
- [clip](clip.md)
- [cls](cls.md)
- [cmd](Cmd.md)
- [cmdkey](cmdkey.md)
- [cmstp](cmstp.md)
- [color](color.md)
- [comp](comp.md)
- [compact](compact.md)
- [Compact vDisk](compact-vdisk.md)
- [convert](convert.md)
  - [convert basic](convert-basic.md)
  - [convert dynamic](convert-dynamic.md)
  - [convert gpt](convert-gpt.md)
  - [convert mbr](convert-mbr.md)
- [copy](copy.md)
- [cprofile](cprofile.md)
- [crear](create.md)
  - [create partition efi](create-partition-efi.md)
  - [crear [partición extendida](create-partition-extended.md)
  - [create partition logical](create-partition-logical.md)
  - [create partition msr](create-partition-msr.md)
  - [create partition primary](create-partition-primary.md)
  - [crear reflejo de volumen](create-volume-mirror.md)
  - [create volume raid](create-volume-raid.md)
  - [create volume simple](create-volume-simple.md)
  - [create volume stripe](create-volume-stripe.md)
- [cscript](cscript.md)

### <a name="d"></a>D

- [date](date.md)
- [dcgpofix](dcgpofix.md)
- [defrag](defrag.md)
- [del](del.md)
- [delete](delete.md)
  - [eliminar disco](delete-disk.md)
  - [delete partition](delete-partition.md)
  - [eliminar sombras](delete-shadows.md)
  - [delete volume](delete-volume.md)
- [detach vdisk](detach-vdisk.md)
- [detalle](detail.md)
  - [detail disk](detail-disk.md)
  - [detail partition](detail-partition.md)
  - [detalles del vDisk](detail-vdisk.md)
  - [detail volume](detail-volume.md)
- [dfsdiag](dfsdiag.md)
  - [dfsdiag testdcs](dfsdiag-testdcs.md)
  - [dfsdiag testdfsconfig](dfsdiag-testdfsconfig.md)
  - [dfsdiag testdfsintegrity](dfsdiag-testdfsintegrity.md)
  - [dfsdiag testreferral](dfsdiag-testreferral.md)
  - [dfsdiag testsites](dfsdiag-testsites.md)
- [dfsrmig](dfsrmig.md)
- [diantz](diantz.md)
- [dir](dir.md)
- [diskcomp](diskcomp.md)
- [diskcopy](diskcopy.md)
- [diskpart](diskpart.md)
- [diskperf](diskperf.md)
- [diskraid](diskraid.md)
- [diskshadow](diskshadow.md)
- [dispdiag](dispdiag.md)
- [DNSCmd](dnscmd.md)
- [doskey](doskey.md)
- [driverquery](driverquery.md)

### <a name="e"></a>E

- [echo](echo.md)
- [edit](edit.md)
- [endlocal](endlocal.md)
- [finalizar restauración](end-restore.md)
- [erase](erase.md)
- [eventcreate](eventcreate.md)
- [eventquery](eventquery.md)
- [eventtriggers](eventtriggers.md)
- [Evntcmd](evntcmd.md)
- [ejec](exec.md)
- [exit](exit_2.md)
- [expand](expand.md)
- [expandir vDisk](expand-vdisk.md)
- [aprovecha](expose.md)
- [extend](extend.md)
- [extract](extract.md)

### <a name="f"></a>F

- [fc](fc.md)
- [filesystems](filesystems.md)
- [find](find.md)
- [findstr](findstr.md)
- [finger](finger.md)
- [flattemp](flattemp.md)
- [fondue](fondue.md)
- [for](for.md)
- [forfiles](forfiles.md)
- [format](format.md)
- [freedisk](freedisk.md)
- [fsutil](fsutil.md)
  - [fsutil 8dot3name](fsutil-8dot3name.md)
  - [fsutil behavior](fsutil-behavior.md)
  - [fsutil dirty](fsutil-dirty.md)
  - [fsutil file](fsutil-file.md)
  - [fsutil fsinfo](fsutil-fsinfo.md)
  - [fsutil hardlink](fsutil-hardlink.md)
  - [fsutil objectid](fsutil-objectid.md)
  - [fsutil quota](fsutil-quota.md)
  - [fsutil repair](fsutil-repair.md)
  - [fsutil reparsepoint](fsutil-reparsepoint.md)
  - [fsutil resource](fsutil-resource.md)
  - [fsutil sparse](fsutil-sparse.md)
  - [fsutil tiering](fsutil-tiering.md)
  - [fsutil transaction](fsutil-transaction.md)
  - [fsutil usn](fsutil-usn.md)
  - [fsutil volume](fsutil-volume.md)
  - [fsutil wim](fsutil-wim.md)
- [ftp](ftp.md)
  - [ftp append](ftp-append.md)
  - [ftp ascii](ftp-ascii.md)
  - [ftp bell](ftp-bell_1.md)
  - [ftp binary](ftp-binary.md)
  - [ftp bye](ftp-bye.md)
  - [ftp cd](ftp-cd.md)
  - [ftp close](ftp-close_1.md)
  - [ftp debug](ftp-debug.md)
  - [ftp delete](ftp-delete.md)
  - [ftp dir](ftp-dir.md)
  - [ftp disconnect](ftp-disconnect_1.md)
  - [ftp get](ftp-get.md)
  - [ftp glob](ftp-glob_1.md)
  - [ftp hash](ftp-hash_1.md)
  - [ftp lcd](ftp-lcd.md)
  - [ftp literal](ftp-literal_1.md)
  - [ftp ls](ftp-ls_1.md)
  - [ftp mget](ftp-mget.md)
  - [ftp mkdir](ftp-mkdir.md)
  - [ftp mls](ftp-mls_1.md)
  - [ftp mput](ftp-mput_1.md)
  - [ftp open](ftp-open_1.md)
  - [ftp prompt](ftp-prompt_1.md)
  - [ftp put](ftp-put.md)
  - [ftp pwd](ftp-pwd_1.md)
  - [ftp quit](ftp-quit.md)
  - [ftp quote](ftp-quote.md)
  - [ftp recv](ftp-recv.md)
  - [ftp remotehelp](ftp-remotehelp_1.md)
  - [ftp rename](ftp-rename.md)
  - [ftp rmdir](ftp-rmdir.md)
  - [ftp send](ftp-send_1.md)
  - [ftp status](ftp-status.md)
  - [ftp trace](ftp-trace_1.md)
  - [ftp type](ftp-type.md)
  - [ftp user](ftp-user.md)
  - [ftp verbose](ftp-verbose_1.md)
  - [ftp mdelete](ftp-.mdelete_1.md)
  - [ftp mdir](ftp.mdir.md)
- [ftype](ftype.md)
- [fveupdate](fveupdate.md)

### <a name="g"></a>G

- [getmac](getmac.md)
- [gettype](gettype.md)
- [goto](goto.md)
- [gpfixup](gpfixup.md)
- [gpresult](gpresult.md)
- [gpt](gpt.md)
- [gpupdate](gpupdate.md)
- [graftabl](graftabl.md)

### <a name="h"></a>H

- [help](help.md)
- [helpctr](helpctr.md)
- [hostname](hostname.md)

### <a name="i"></a>I

- [icacls](icacls.md)
- [if](if.md)
- [importar (shadowdisk)](import.md)
- [importar (DiskPart)](import_1.md)
- [inactive](inactive.md)
- [inuse](inuse.md)
- [ipconfig](ipconfig.md)
- [ipxroute](ipxroute.md)
- [irftp](irftp.md)

### <a name="j"></a>J

- [jetpack](jetpack.md)

### <a name="k"></a>K

- [klist](klist.md)
- [ksetup](ksetup.md)
  - [ksetup addenctypeattr](ksetup-addenctypeattr.md)
  - [ksetup addhosttorealmmap](ksetup-addhosttorealmmap.md)
  - [ksetup addkdc](ksetup-addkdc.md)
  - [ksetup addkpasswd](ksetup-addkpasswd.md)
  - [ksetup addrealmflags](ksetup-addrealmflags.md)
  - [ksetup changepassword](ksetup-changepassword.md)
  - [ksetup delenctypeattr](ksetup-delenctypeattr.md)
  - [ksetup delhosttorealmmap](ksetup-delhosttorealmmap.md)
  - [ksetup delkdc](ksetup-delkdc.md)
  - [ksetup delkpasswd](ksetup-delkpasswd.md)
  - [ksetup delrealmflags](ksetup-delrealmflags.md)
  - [ksetup domain](ksetup-domain.md)
  - [ksetup dumpstate](ksetup-dumpstate.md)
  - [ksetup getenctypeattr](ksetup-getenctypeattr.md)
  - [ksetup listrealmflags](ksetup-listrealmflags.md)
  - [ksetup mapuser](ksetup-mapuser.md)
  - [ksetup removerealm](ksetup-removerealm.md)
  - [ksetup server](ksetup-server.md)
  - [ksetup setcomputerpassword](ksetup-setcomputerpassword.md)
  - [ksetup setenctypeattr](ksetup-setenctypeattr.md)
  - [ksetup setrealm](ksetup-setrealm.md)
  - [ksetup setrealmflags](ksetup-setrealmflags.md)
- [ktmutil](ktmutil.md)
- [ktpass](ktpass.md)

### <a name="l"></a>L

- [label](label.md)
- [list](list.md)
  - [enumerar proveedores](list-providers.md)
  - [Mostrar sombras](list-shadows.md)
  - [enumerar escritores](list-writers.md)
- [cargar metadatos](load-metadata.md)
- [lodctr](lodctr.md)
- [logman](logman.md)
  - [logman create](logman-create.md)
  - [Logman crear alerta](logman-create-alert.md)
  - [Logman Create API](logman-create-api.md)
  - [Logman crear cfg](logman-create-cfg.md)
  - [Logman Create Counter](logman-create-counter.md)
  - [Logman crear seguimiento](logman-create-trace.md)
  - [logman delete](logman-delete.md)
  - [Logman Import y Logman Export](logman-import-export.md)
  - [logman query](logman-query.md)
  - [Logman Start y Logman STOP](logman-start-stop.md)
  - [logman update](logman-update.md)
  - [alerta de actualización de Logman](logman-update-alert.md)
  - [Logman Update API](logman-update-api.md)
  - [Logman Update cfg](logman-update-cfg.md)
  - [Logman (actualizar contador)](logman-update-counter.md)
  - [Logman actualizar seguimiento](logman-update-trace.md)
- [logoff](logoff.md)
- [lpq](lpq.md)
- [lpr](lpr.md)

### <a name="m"></a>M

- [macfile](macfile.md)
- [makecab](makecab.md)
- [administrar BDE](manage-bde.md)
  - [administrar el estado de BDE](manage-bde-status.md)
  - [administrar BDE en](manage-bde-on.md)
  - [administrar BDE desactivado](manage-bde-off.md)
  - [administrar la pausa de BDE](manage-bde-pause.md)
  - [administrar BDE resume](manage-bde-resume.md)
  - [administrar bloqueo BDE](manage-bde-lock.md)
  - [administrar el desbloqueo de BDE](manage-bde-unlock.md)
  - [administrar el desbloqueo de BDE](manage-bde-autounlock.md)
  - [administrar protectores de BDE](manage-bde-protectors.md)
  - [administrar el TPM de BDE](manage-bde-tpm.md)
  - [administrar BDE setidentifier](manage-bde-setidentifier.md)
  - [administrar BDE forcerecovery](manage-bde-forcerecovery.md)
  - [administrar BDE ChangePassword](manage-bde-changepassword.md)
  - [administrar BDE changepin](manage-bde-changepin.md)
  - [administrar BDE changekey](manage-bde-changekey.md)
  - [administrar BDE keypackage](manage-bde-keypackage.md)
  - [administrar la actualización de BDE](manage-bde-upgrade.md)
  - [administrar BDE wipefreespace](manage-bde-wipefreespace.md)
- [mapadmin](mapadmin.md)
- [md](md.md)
- [Merge vDisk](merge-vdisk.md)
- [mkdir](mkdir.md)
- [mklink](mklink.md)
- [mmc](mmc.md)
- [mode](mode.md)
- [more](more.md)
- [mount](mount.md)
- [mountvol](mountvol.md)
- [move](move.md)
- [mqbkup](mqbkup.md)
- [mqsvc](mqsvc.md)
- [mqtgsvc](mqtgsvc.md)
- [msdt](msdt.md)
- [msg](msg.md)
- [msiexec](msiexec.md)
- [msinfo32](msinfo32.md)
- [mstsc](mstsc.md)

### <a name="n"></a>N

- [nbtstat](nbtstat.md)
- [netcfg](netcfg.md)
- [impresión en red](net-print.md)
- [netsh](netsh.md)
- [netstat](netstat.md)
- [nfsadmin](nfsadmin.md)
- [nfsshare](nfsshare.md)
- [nfsstat](nfsstat.md)
- [nlbmgr](nlbmgr.md)
- [nslookup](nslookup.md)
  - [Comando nslookup exit](nslookup-exit-command.md)
  - [Comando nslookup finger](nslookup-finger-command.md)
  - [nslookup help](nslookup-help.md)
  - [nslookup ls](nslookup-ls.md)
  - [nslookup lserver](nslookup-lserver.md)
  - [nslookup root](nslookup-root.md)
  - [nslookup server](nslookup-server.md)
  - [nslookup set](nslookup-set.md)
  - [nslookup set all](nslookup-set-all.md)
  - [nslookup set class](nslookup-set-class.md)
  - [nslookup set d2](nslookup-set-d2.md)
  - [nslookup set debug](nslookup-set-debug.md)
  - [nslookup set domain](nslookup-set-domain.md)
  - [nslookup set port](nslookup-set-port.md)
  - [nslookup set querytype](nslookup-set-querytype.md)
  - [nslookup set recurse](nslookup-set-recurse.md)
  - [nslookup set retry](nslookup-set-retry.md)
  - [nslookup set root](nslookup-set-root.md)
  - [nslookup set search](nslookup-set-search.md)
  - [nslookup set srchlist](nslookup-set-srchlist.md)
  - [nslookup set timeout](nslookup-set-timeout.md)
  - [nslookup set type](nslookup-set-type.md)
  - [nslookup set vc](nslookup-set-vc.md)
  - [nslookup view](nslookup-view.md)
- [ntbackup](ntbackup.md)
- [ntcmdprompt](ntcmdprompt.md)
- [ntfrsutl](ntfrsutl.md)

### <a name="o"></a>O

- [n](offline.md)
  - [disco sin conexión](offline-disk.md)
  - [volumen sin conexión](offline-volume.md)
- [pantalla](online.md)
  - [disco en línea](online-disk.md)
  - [volumen en línea](online-volume.md)
- [openfiles](openfiles.md)

### <a name="p"></a>P

- [pagefileconfig](pagefileconfig.md)
- [path](path.md)
- [pathping](pathping.md)
- [pause](pause.md)
- [pbadmin](pbadmin.md)
- [pentnt](pentnt.md)
- [perfmon](perfmon.md)
- [ping](ping.md)
- [pnpunattend](pnpunattend.md)
- [pnputil](pnputil.md)
- [popd](popd.md)
- [PowerShell](powershell.md)
- [PowerShell ISE](powershell_ise.md)
- [print](print.md)
- [prncnfg](prncnfg.md)
- [prndrvr](prndrvr.md)
- [prnjobs](prnjobs.md)
- [prnmngr](prnmngr.md)
- [prnport](prnport.md)
- [prnqctl](prnqctl.md)
- [prompt](prompt.md)
- [pubprn](pubprn.md)
- [pushd](pushd.md)
- [pushprinterconnections](pushprinterconnections.md)
- [pwlauncher](pwlauncher.md)

### <a name="q"></a>Q

- [qappsrv](qappsrv.md)
- [qprocess](qprocess.md)
- [consulta](query.md)
  - [query process](query-process.md)
  - [query session](query-session.md)
  - [consulta termserver](query-termserver.md)
  - [query user](query-user.md)
- [quser](quser.md)
- [qwinsta](qwinsta.md)

### <a name="r"></a>R

- [rcp](rcp.md)
- [rd](rd.md)
- [rdpsign](rdpsign.md)
- [recover](recover.md)
- [recuperar grupo de discos](recover_1.md)
- [reg](reg.md)
  - [REG Add](reg-add.md)
  - [REG-comparar](reg-compare.md)
  - [copiar reg](reg-copy.md)
  - [Eliminar registro](reg-delete.md)
  - [REG Export](reg-export.md)
  - [REG Import](reg-import.md)
  - [carga de reg](reg-load.md)
  - [REG (consulta)](reg-query.md)
  - [REG restore](reg-restore.md)
  - [Guardar registro](reg-save.md)
  - [descarga de reg](reg-unload.md)
- [regini](regini.md)
- [regsvr32](regsvr32.md)
- [relog](relog.md)
- [Rem archivo por lotes](rem.md)
- [Rem script](rem_1.md)
- [remove](remove.md)
- [ren](ren.md)
- [rename](rename.md)
- [reparación](repair.md)
  - [reparar BDE](repair-bde.md)
- [replace](replace.md)
- [volver a examinar](rescan.md)
- [reset](reset.md)
  - [reset session](reset-session.md)
- [Tain](retain.md)
- [revertir](revert.md)
- [rexec](rexec.md)
- [risetup](risetup.md)
- [rmdir](rmdir.md)
- [robocopy](robocopy.md)
- [Ruta ws2008](route_ws2008.md)
- [rpcinfo](rpcinfo.md)
- [rpcping](rpcping.md)
- [rsh](rsh.md)
- [rundll32](rundll32.md)
- [printui de rundll32](rundll32-printui.md)
- [rwinsta](rwinsta.md)

### <a name="s"></a>S

- [red](san.md)
- [configuración de SC](sc-config.md)
- [creación de SC](sc-create.md)
- [eliminación de SC](sc-delete.md)
- [consulta SC](sc-query.md)
- [schtasks](schtasks.md)
- [scwcmd](scwcmd.md)
  - [scwcmd ANALYZE](scwcmd-analyze.md)
  - [scwcmd configure](scwcmd-configure.md)
  - [scwcmd registrar](scwcmd-register.md)
  - [reversión de scwcmd](scwcmd-rollback.md)
  - [scwcmd (transformación)](scwcmd-transform.md)
  - [vista scwcmd](scwcmd-view.md)
- [secedit](secedit.md)
  - [secedit ANALYZE](secedit-analyze.md)
  - [configuración de secedit](secedit-configure.md)
  - [exportación de secedit](secedit-export.md)
  - [secedit generaterollback](secedit-generaterollback.md)
  - [importación de secedit](secedit-import.md)
  - [validar secedit](secedit-validate.md)
- [select](select.md)
  - [select disk](select-disk.md)
  - [select partition](select-partition.md)
  - [seleccionar vDisk](select-vdisk.md)
  - [select volume](select-volume.md)
- [serverceipoptin](serverceipoptin.md)
- [ServerManagerCmd](servermanagercmd.md)
- [serverweroptin](serverweroptin.md)
- [establecer variables de entorno](set_1.md)
- [establecer instantánea](set_2.md)
  - [establecer contexto](set-context.md)
  - [identificador de conjunto](set-id.md)
  - [setlocal](setlocal.md)
  - [establecer metadatos](set-metadata.md)
  - [opción set](set-option.md)
  - [establecer detallado](set-verbose.md)
- [setx](setx.md)
- [sfc](sfc.md)
- [shadow](shadow.md)
- [shift](shift.md)
- [showmount](showmount.md)
- [shrink](shrink.md)
- [shutdown](shutdown.md)
- [simular restauración](simulate-restore.md)
- [sort](sort.md)
- [start](start.md)
- [subcomando (conjunto de dispositivos)](subcommand-set-device.md)
- [subcomando set drivergroup](subcommand-set-drivergroup.md)
- [subcomando set drivergroupfilter](subcommand-set-drivergroupfilter.md)
- [subcomando set driverpackage](subcommand-set-driverpackage.md)
- [imagen del conjunto de subcomandos](subcommand-set-image.md)
- [subcomando set imagegroup](subcommand-set-imagegroup.md)
- [servidor de conjunto de subcomandos](subcommand-set-server.md)
- [subcomando set TransportServer](subcommand-set-transportserver.md)
- [subcomando set multicasttransmission](subcommand-start-multicasttransmission.md)
- [espacio de nombres de subcomando Start](subcommand-start-namespace.md)
- [servidor de inicio de subcomando](subcommand-start-server.md)
- [subcomando Start TransportServer](subcommand-start-transportserver.md)
- [subcomando detener servidor](subcommand-stop-server.md)
- [Subcommand STOP TransportServer](subcommand-stop-transportserver.md)
- [subst](subst.md)
- [sxstrace](sxstrace.md)
- [sysocmgr](sysocmgr.md)
- [systeminfo](systeminfo.md)


### <a name="t"></a>T

- [takeown](takeown.md)
- [tapicfg](tapicfg.md)
- [taskkill](taskkill.md)
- [tasklist](tasklist.md)
- [tcmsetup](tcmsetup.md)
- [telnet](telnet.md)
  - [cerrar Telnet](telnet-close.md)
  - [pantalla de Telnet](telnet-display.md)
  - [Telnet abierto](telnet-open.md)
  - [cerrar Telnet](telnet-quit.md)
  - [envío de Telnet](telnet-send.md)
  - [Telnet set](telnet-set.md)
  - [Estado de Telnet](telnet-status.md)
  - [no establecido Telnet](telnet-unset.md)
- [tftp](tftp.md)
- [time](time.md)
- [timeout](timeout_1.md)
- [title](title_1.md)
- [tlntadmn](tlntadmn.md)
- [tpmtool](tpmtool.md)
- [tpmvscmgr](tpmvscmgr.md)
- [tracerpt](tracerpt_1.md)
- [tracert](tracert.md)
- [tree](tree.md)
- [tscon](tscon.md)
- [tsdiscon](tsdiscon.md)
- [tsecimp](tsecimp_1.md)
- [tskill](tskill.md)
- [tsprof](tsprof.md)
- [type](type.md)
- [typeperf](typeperf.md)
- [tzutil](tzutil.md)

### <a name="u"></a>U

- [x:.](unexpose.md)
- [UniqueID](uniqueid.md)
- [unlodctr](unlodctr_1.md)

### <a name="v"></a>V

- [ver](ver.md)
- [verifier](verifier.md)
- [verify](verify_1.md)
- [vol](vol.md)
- [vssadmin](vssadmin.md)
  - [vssadmin delete shadows](vssadmin-delete-shadows.md)
  - [vssadmin list shadows](vssadmin-list-shadows.md)
  - [vssadmin list writers](vssadmin-list-writers.md)
  - [vssadmin resize shadowstorage](vssadmin-resize-shadowstorage.md)

### <a name="w"></a>W

- [waitfor](waitfor.md)
- [wbadmin](wbadmin.md)
  - [Wbadmin Delete Catalog](wbadmin-delete-catalog.md)
  - [Wbadmin Delete systemstatebackup](wbadmin-delete-systemstatebackup.md)
  - [Wbadmin deshabilitar copia de seguridad](wbadmin-disable-backup.md)
  - [Wbadmin habilitar copia de seguridad](wbadmin-enable-backup.md)
  - [obtener discos de Wbadmin](wbadmin-get-disks.md)
  - [elementos get de Wbadmin](wbadmin-get-items.md)
  - [Estado de Wbadmin get](wbadmin-get-status.md)
  - [versiones de Wbadmin get](wbadmin-get-versions.md)
  - [Wbadmin restore Catalog](wbadmin-restore-catalog.md)
  - [copia de seguridad de inicio de Wbadmin](wbadmin-start-backup.md)
  - [recuperación de inicio de Wbadmin](wbadmin-start-recovery.md)
  - [Wbadmin Start sysrecovery](wbadmin-start-sysrecovery.md)
  - [Inicio de systemstatebackup de Wbadmin](wbadmin-start-systemstatebackup.md)
  - [Inicio de systemstaterecovery de Wbadmin](wbadmin-start-systemstaterecovery.md)
  - [trabajo de detención de Wbadmin](wbadmin-stop-job.md)
- [wdsutil](wdsutil.md)
- [wecutil](wecutil.md)
- [wevtutil](wevtutil.md)
- [where](where_1.md)
- [whoami](whoami.md)
- [winnt](winnt.md)
- [winnt32](winnt32.md)
- [winpop](winpop.md)
- [winrs](winrs.md)
- [memoria de WinSAT](winsat-mem.md)
- [Mfmedia de WinSAT](winsat-mfmedia.md)
- [wmic](wmic.md)
- [escritor](writer.md)
- [wscript](wscript.md)

### <a name="x"></a>X

- [xcopy](xcopy.md)

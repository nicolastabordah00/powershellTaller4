## EJERCICIOS
1. Mostrar una tabla de procesos que incluya únicamente los nombres de los
   procesos, sus IDs, y si están respondiendo a Windows (la propiedad
   ``Responding`` muestra eso). Haga que la tabla tome el mínimo de espacio
   horizontal, pero no permita que la información se trunque.
   ```console
   PS C:\Users\Nicolas> Get-Process | Select Name, Id, Responding | ft -Wrap
   ```
   ```
   Name                                           Id Responding
   ----                                           -- ----------
   AAM Updates Notifier                         6516       True
   AAM Updates Notifier                        10480       True
   AppleMobileDeviceService                     4028       True
   ApplicationFrameHost                        11056       True
   audiodg                                     15456       True
   browser_broker                              11200       True
   BtwRSupportService                           4060       True
   ``` 
2. Muestre una tabla de procesos que incluya los nombres de los procesos y sus
   IDs. También incluya columnas para uso de memoria virtual y física;
   exprese dichos valores en megabytes (MB).
   ```console
   PS C:\Users\Nicolas> Get-Process | ft Name, Id, @{n='VM (MB)';e={$_.VM / 1MB -as [int]}}, @{n='PM (MB)'; e={$_.PM / 1MB -as [int]}}
   ```
   ```
   Name                                                              Id VM (MB) PM (MB)
   ----                                                              -- ------- -------
   AAM Updates Notifier                                            6516     134       5
   AAM Updates Notifier                                           10480       6       0
   AppleMobileDeviceService                                        4028    4242       4
   ```
3. Emplee ``Get-EventLog`` para mostrar una lista de los logs de eventos
   disponibles (revise la ayuda para encontrar el parámetro que le permitirá
   obtener dicha información). Formatee la salida como una tabla que incluya
   el nombre de despliegue del log y el período de retención. Los encabezados
   de columna deben ser NombreLog y Per-Retencion.
   ```console
   PS C:\Users\Nicolas> Get-EventLog -List | ft @{n='NombreLog';e={$_.LogDisplayName}}, @{n='Per-Retencion';e={$_.minimumRetentionDays}}
   ```
   ```
   NombreLog                              Per-Retencion
   -------                              ----------------
   Aplicación                                          0
   Eventos de hardware                                 0
   Hewlett-Packard                                     7
   HP 3D DriveGuard                                    7
   HP Software Framework                               7
   Internet Explorer                                   7
   Servicio de administración de claves                0
   Microsoft Office Alerts                             0
                                                     
   Sistema                                             0
   Windows Azure                                       7
   Windows PowerShell                                  0
   ```

4. Muestre una lista de servicios, de tal manera que aparezcan agrupados los
   servicios que están iniciados y los que están detenidos. Los que están
   iniciados deben aparecer primero.
   ```console
   PS C:\Users\Nicolas> Get-Service | sort Status | fl -GroupBy status
   ```
   ```
   Status: Stopped


    Name                : AarSvc_104fce
    DisplayName         : Agent Activation Runtime_104fce
    Status              : Stopped
    DependentServices   : {}
    ServicesDependedOn  : {}
    CanPauseAndContinue : False
    CanShutdown         : False
    CanStop             : False
    ServiceType         : 224

    Status: Running


    Name                : ClickToRunSvc
    DisplayName         : Servicio Hacer clic y ejecutar de Microsoft Office
    Status              : Running
    DependentServices   : {}
    ServicesDependedOn  : {}
    CanPauseAndContinue : False
    CanShutdown         : True
    CanStop             : True
    ServiceType         : Win32OwnProcess
   ```

5. Mostrar una lista a cuatro columnas de todos los directorios que están en
   el raíz de la unidad ``C:``
   ```console
   PS C:\Users\Nicolas> ls -Path C:\ | fw -Column 4
   ```
   ```
   Directorio: C:\



    inetpub                                 Intel                                   intelFPGA                               PerfLogs                               
    Program Files                           Program Files (x86)                     semana3                                 SWSetup                                
    Users                                   Windows                                 Windows.old                             IFRToolLog.txt                         
    rescue.info                             session.log                             spssprod.inf                                        
   ```

6. Cree una lista formateada de todos los archivos ``.exe`` del directorio
   ``C:\Windows``. Debe mostrarse el nombre, la información de versión, y el
   tamaño del archivo. La propiedad de tamaño se llama ``length`` en Powershell,
   pero para mayor claridad, la columna se debe llamar **Tamaño** en su listado.
   ```console
   PS C:\Users\Nicolas> Get-ChildItem -Path C:\Windows | where -filter {$_.Name -like "*.exe"} | fl -Property Name, VersionInfo, @{n='Tamaño';e={$_.length}}
   ```


7. Importe el módulo ``NetAdapter`` (empleando el comando ``Import-Module
   NetAdapter``).
   Empleando el cmdlet ``Get-NetAdapter``, muestre una lista de adaptadores no
   virtuales (adaptadores cuya propiedad Virtual sea falsa. El valor lógico
   falso es representado por Powershell como ``$False``).
   ```console
   PS C:\Users\Nicolas> Get-NetAdapter | where -filter {$_.Virtual -eq $false} | fl
   ```


8. Importe el módulo ``DnsClient``. Empleando el cmdlet ``Get-DnsClientCache``,
   muestre una lista de los registros ``A`` y ``AAAA`` que estén en el caché.
   Sugerencia: Si el caché está vacío, visite algunos sitios web para poblarlo.
   ```console
   Get-DnsClientCache | where -filter {$_.Type -eq (1) -or $_.Type -eq (28)} | fl
   ```


9. Genere una lista de todos los archivos ``.exe`` del directorio
   ``C:\Windows\System32`` que tengan más de 5 MB.
   ```console
   PS C:\Users\Nicolas> Get-ChildItem -Path C:\Windows\System32 | where -filter {$_.Name -like "*.exe" -and $_.length/1MB -gt 5} | fl
   ```


10. Muestre una lista de parches que sean actualizaciones de seguridad.
   ```console
   PS C:\Users\Nicolas> Get-HotFix | where -filter {$_.description -like "*Security*"} |fl
   ```


11. Muestre una lista de parches que hayan sido instalados por el
    usuario ``Administrador``, que sean actualizaciones. Si no tiene ninguno,
    busque parches instalados por el usuario ``System``. Note que algunos parches
    no tienen valor en el campo ``Installed By``.
    ```console
    PS C:\Users\Nicolas> Get-HotFix | where -filter {$_.installedBy -like "*SYSTEM*"} |fl
    ```


12. Genere una lista de todos los procesos que estén corriendo con el nombre
    **Conhost** o **Svchost**.
    ```console
    PS C:\Users\Nicolas> Get-Process | where -filter {$_.Name -eq "Conhost" -or $_.Name -eq "Svchost"} | fl
    ```
  

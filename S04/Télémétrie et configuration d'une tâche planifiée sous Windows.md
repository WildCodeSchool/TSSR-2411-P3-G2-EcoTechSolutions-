# I) Télémétrie et configuration d'une tâche planifiée sous Windows Server 2022


---

## 1. Création d'une nouvelle tâche

1. Ouvrir le **Planificateur de tâches** en tapant `taskschd.msc` dans la barre de recherche Windows.
2. Cliquer sur **Créer une tâche**.
3. Dans l'onglet **Général** :
   - Entrer un nom pour la tâche, ici **CollectTelemetryDaily**.
   - Spécifier l'utilisateur sous lequel la tâche s'exécutera (**ECOTECHSOLUTION\Administrator**).
   - Cocher **Exécuter avec les autorisations maximales**.

![Création d'une tâche](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S04/T%C3%A9l%C3%A9m%C3%A9trie/01_Cr%C3%A9ation_tache.PNG)


---

## 2. Définition du déclencheur

1. Aller dans l'onglet **Déclencheurs** et cliquer sur **Nouveau**.
2. Sélectionner **À l'heure programmée**.
3. Spécifier une date de début et une heure.
4. Sélectionner **Répéter tous les 1 jours**.
5. Cocher **Activée**.
6. Valider avec **OK**.

![Définition du déclencheur](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S04/T%C3%A9l%C3%A9m%C3%A9trie/02_Definir_D%C3%A9clencheur.PNG
)

Une fois enregistré, le déclencheur apparaîtra dans la liste :

![Liste des déclencheurs](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S04/T%C3%A9l%C3%A9m%C3%A9trie/03_Definir_D%C3%A9clencheur2.PNG
)
---

## 3. Définition de l'action à exécuter

1. Aller dans l'onglet **Actions** et cliquer sur **Nouveau**.
2. Sélectionner **Démarrer un programme**.
3. Dans le champ **Programme/script**, entrer `powershell.exe`.
4. Dans **Ajouter des arguments**, entrer `-ExecutionPolicy Bypass`.
5. Valider avec **OK**.

![Définition de l'action](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S04/T%C3%A9l%C3%A9m%C3%A9trie/04_Definir_Action.PNG
)
---

## 4. Configuration des paramètres avancés

1. Aller dans l'onglet **Paramètres**.
2. Cocher **Autoriser l'exécution de la tâche à la demande**.
3. Cocher **Exécuter la tâche dès que possible si un démarrage planifié est manqué**.
4. Définir une durée maximale d'exécution (ex: **3 jours**).
5. Sélectionner **Ne pas démarrer une nouvelle instance** si la tâche est déjà en cours.
6. Valider avec **OK**.

![Paramètres avancés](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S04/T%C3%A9l%C3%A9m%C3%A9trie/05_Param%C3%A8tres_optionnels.PNG
)
---

# II) Description du Script PowerShell de Collecte de Télémétrie

Ce script PowerShell collecte diverses informations sur l'état du système et les enregistre dans des fichiers CSV. Il permet de surveiller l'utilisation des ressources (CPU, mémoire, disque, réseau), les journaux d'événements, l'état des services critiques, les mises à jour Windows et l'antivirus.

---

## 1. Paramètres du script

- **$RootLogDir** : Dossier où seront stockés les fichiers CSV.
- **$TimeStamp** : Horodatage pour nommer les fichiers CSV.
- **$CpuThreshold** et **$DiskThreshold** : Seuils de surveillance pour CPU et espace disque libre.
- **$CriticalServices** : Liste des services critiques à surveiller.
- **$EventLogsToCollect** : Journaux d'événements à collecter.
- **$EventTimeSpan** : Durée pour laquelle les événements seront extraits (1 jour).

---

## 2. Fonctions principales

### Collecte des Ressources Système
- **Get-CPUUsage** : Récupère l'utilisation du CPU en pourcentage.
- **Get-MemoryUsage** : Récupère l'utilisation de la mémoire vive en pourcentage.
- **Get-DiskUsage** : Récupère les informations sur l'utilisation des disques (total, libre, pourcentage d'occupation).
- **Get-NetworkUsage** : Collecte les octets reçus et envoyés sur les interfaces réseau.

### Collecte des Informations du Système
- **Get-EventLogs** : Extrait les journaux d'événements Windows (Application, Système) sur les dernières 24 heures.
- **Get-WindowsUpdateInfo** : Récupère la liste des dernières mises à jour installées.
- **Get-AntivirusInfo** : Obtient les informations sur l'antivirus via SecurityCenter2.
- **Check-ServiceState** : Vérifie l'état des services critiques définis dans **$CriticalServices**.

---

## 3. Collecte et Analyse des Données

- Exécute les fonctions de collecte (CPU, mémoire, disque, réseau, journaux, services, antivirus, Windows Update).
- Affiche des alertes en cas de dépassement des seuils prédéfinis (CPU trop élevé, espace disque faible).

---

## 4. Enregistrement des Données en CSV

Les informations collectées sont enregistrées dans des fichiers CSV distincts dans **$RootLogDir** :

1. **SystemUsage_yyyyMMdd_HHmmss.csv** : Utilisation CPU et mémoire.
2. **DiskUsage_yyyyMMdd_HHmmss.csv** : Usage des disques.
3. **Events_yyyyMMdd_HHmmss.csv** : Journaux d'événements.
4. **WindowsUpdate_yyyyMMdd_HHmmss.csv** : Mises à jour installées.
5. **Antivirus_yyyyMMdd_HHmmss.csv** : Informations sur l'antivirus.
6. **Services_yyyyMMdd_HHmmss.csv** : État des services critiques.
7. **NetworkUsage_yyyyMMdd_HHmmss.csv** : Statistiques réseau.

---

## 5. Script Powershell

<pre>



# -------------------------- SECTION 1 : Paramètres du script --------------------------

# 1. Dossier de base où stocker les CSV
$RootLogDir  = "C:\Logs\Telemetry"

# 2. Génère un suffixe horodaté (année-mois-jour_heure-minute-seconde)
$TimeStamp   = (Get-Date -Format 'yyyyMMdd_HHmmss')

# 3. Seuils (facultatifs, utilisés si tu veux faire de la vérification locale)
$CpuThreshold   = 80   # en %
$DiskThreshold  = 10   # % d’espace disque libre minimum

# 4. Services critiques à surveiller
$CriticalServices = @(
    "Spooler",     # Exemple: service d'impression
    "wuauserv",    # Service Windows Update
    "WinDefend"    # Service Antivirus Windows Defender
)

# 5. Journaux d’événements à collecter
$EventLogsToCollect = @("Application", "System")
# Période sur laquelle récupérer les événements (ex. 1 jour)
$EventTimeSpan = (New-TimeSpan -Days 1)

# ----------------------------- SECTION 2 : Fonctions -----------------------------

function Get-CPUUsage {
    # Collecte usage CPU via WMI/CIM
    $cpu = Get-CimInstance -ClassName Win32_PerfFormattedData_PerfOS_Processor -Filter "Name='_Total'" -ErrorAction SilentlyContinue
    if ($cpu) {
        return [math]::Round($cpu.PercentProcessorTime, 2)
    }
    else {
        return 0  # en cas d’erreur
    }
}

function Get-MemoryUsage {
    # Collecte usage RAM en %
    $os       = Get-CimInstance Win32_OperatingSystem
    $totalMem = $os.TotalVisibleMemorySize  # en KB
    $freeMem  = $os.FreePhysicalMemory      # en KB
    $usedMem  = $totalMem - $freeMem
    $percentUsed = [math]::Round(($usedMem / $totalMem) * 100, 2)
    return $percentUsed
}

function Get-DiskUsage {
    # Retourne un tableau d’objets: lettre du disque, espace total, libre, % used, etc.
    $drives = Get-CimInstance Win32_LogicalDisk -Filter "DriveType=3"
    $diskUsageInfo = foreach ($d in $drives) {
        $size = $d.Size
        if ($size -gt 0) {
            $free         = $d.FreeSpace
            $usedSpace    = $size - $free
            $percentUsed  = [math]::Round(($usedSpace / $size) * 100, 2)
            $percentFree  = 100 - $percentUsed

            [PSCustomObject]@{
                Drive       = $d.DeviceID
                SizeGB      = [math]::Round($size / 1GB, 2)
                FreeSpaceGB = [math]::Round($free / 1GB, 2)
                PercentUsed = $percentUsed
                PercentFree = $percentFree
            }
        }
    }
    return $diskUsageInfo
}

function Get-NetworkUsage {
    # Exemple simple: octets reçus/envoyés sur les interfaces actives
    $netAdapters = Get-NetAdapterStatistics | Where-Object { $_.ReceivedBytes -gt 0 -or $_.SentBytes -gt 0 }
    $netInfo = foreach ($adapter in $netAdapters) {
        [PSCustomObject]@{
            Name          = $adapter.Name
            ReceivedBytes = $adapter.ReceivedBytes
            SentBytes     = $adapter.SentBytes
        }
    }
    return $netInfo
}

function Get-EventLogs {
    param(
        [string[]]$Logs,
        [TimeSpan]$TimeSpan
    )
    $startTime = (Get-Date).Add(-$TimeSpan)
    $allEvents = @()

    foreach ($log in $Logs) {
        $events = Get-EventLog -LogName $log -After $startTime -ErrorAction SilentlyContinue
        if ($events) {
            $allEvents += $events
        }
    }
    return $allEvents
}

function Get-WindowsUpdateInfo {
    # Récupération infos mises à jour
    Get-CimInstance Win32_QuickFixEngineering
}

function Get-AntivirusInfo {
    # Récupération infos Antivirus via SecurityCenter2
    Get-CimInstance -Namespace "root\SecurityCenter2" -ClassName "AntiVirusProduct" -ErrorAction SilentlyContinue
}

function Check-ServiceState {
    param(
        [string[]]$Services
    )
    $serviceStates = @()
    foreach ($svc in $Services) {
        $serviceObj = Get-Service -Name $svc -ErrorAction SilentlyContinue
        if ($serviceObj) {
            $serviceStates += [PSCustomObject]@{
                ServiceName = $svc
                Status      = $serviceObj.Status
            }
        }
        else {
            $serviceStates += [PSCustomObject]@{
                ServiceName = $svc
                Status      = "NotFound"
            }
        }
    }
    return $serviceStates
}

# ------------------------- SECTION 3 : Collecte de la télémétrie -------------------------
Write-Host "Début de la collecte de télémétrie..."

$timestamp    = Get-Date
$cpuUsage     = Get-CPUUsage
$memUsage     = Get-MemoryUsage
$diskUsage    = Get-DiskUsage
$netUsage     = Get-NetworkUsage
$events       = Get-EventLogs -Logs $EventLogsToCollect -TimeSpan $EventTimeSpan
$wuInfo       = Get-WindowsUpdateInfo
$avInfo       = Get-AntivirusInfo
$svcStates    = Check-ServiceState -Services $CriticalServices

Write-Host "Collecte terminée."

# ------------------ SECTION 4 : Vérification de seuils (sans envoi d’e-mails) -----------------
# Exemple : Tu peux conserver la logique si besoin, ou la supprimer si pas utile
if ($cpuUsage -gt $CpuThreshold) {
    Write-Host "ALERTE: CPU élevé ($cpuUsage%) [Seuil=$CpuThreshold%]"
}

foreach ($disk in $diskUsage) {
    if ($disk.PercentFree -lt $DiskThreshold) {
        Write-Host "ALERTE: Espace disque faible sur $($disk.Drive) [Seuil=$DiskThreshold%]"
    }
}

# ------------------ SECTION 5 : Sauvegarde dans fichiers CSV distincts -------------------
# Créer le répertoire si besoin
if (!(Test-Path -Path $RootLogDir)) {
    New-Item -ItemType Directory -Path $RootLogDir | Out-Null
}

#
# 1) Usage Système (CPU/Mémoire)
#
$SystemUsage = [PSCustomObject]@{
    Timestamp   = $timestamp
    CPUPercent  = $cpuUsage
    MemPercent  = $memUsage
}
$systemUsageFile = Join-Path $RootLogDir ("SystemUsage_{0}.csv" -f $TimeStamp)
$SystemUsage | ConvertTo-Csv -NoTypeInformation | Out-File $systemUsageFile -Append

#
# 2) Usage Disques
#
if ($diskUsage) {
    $diskUsageFile = Join-Path $RootLogDir ("DiskUsage_{0}.csv" -f $TimeStamp)
    $diskUsage | ConvertTo-Csv -NoTypeInformation | Out-File $diskUsageFile -Append
}

#
# 3) Journaux d’événements
#
if ($events) {
    $eventsFile = Join-Path $RootLogDir ("Events_{0}.csv" -f $TimeStamp)
    $events | Select-Object EventID, Source, EntryType, TimeGenerated, Message |
        ConvertTo-Csv -NoTypeInformation | Out-File $eventsFile -Append
}

#
# 4) Windows Update
#
if ($wuInfo) {
    $wuFile = Join-Path $RootLogDir ("WindowsUpdate_{0}.csv" -f $TimeStamp)
    $wuInfo | Select-Object HotFixID, InstalledOn, Description |
        ConvertTo-Csv -NoTypeInformation | Out-File $wuFile -Append
}

#
# 5) Antivirus
#
if ($avInfo) {
    $avFile = Join-Path $RootLogDir ("Antivirus_{0}.csv" -f $TimeStamp)
    $avInfo | Select-Object displayName, instanceGuid, pathToSignedProductExe, productUptoDate |
        ConvertTo-Csv -NoTypeInformation | Out-File $avFile -Append
}

#
# 6) Services Critiques
#
if ($svcStates) {
    $svcFile = Join-Path $RootLogDir ("Services_{0}.csv" -f $TimeStamp)
    $svcStates | Select-Object ServiceName, Status |
        ConvertTo-Csv -NoTypeInformation | Out-File $svcFile -Append
}

#
# 7) Usage Réseau
#
if ($netUsage) {
    $netFile = Join-Path $RootLogDir ("NetworkUsage_{0}.csv" -f $TimeStamp)
    $netUsage | ConvertTo-Csv -NoTypeInformation | Out-File $netFile -Append
}

Write-Host "`nLes différentes données ont été enregistrées dans : $RootLogDir"
Write-Host "Fin du script de télémétrie."

  
</pre>






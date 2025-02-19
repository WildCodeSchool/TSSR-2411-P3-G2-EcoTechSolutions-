# Mise en place de LVM sur un serveur Debian

## 1. Vérifier les disques disponibles

Avant de configurer LVM, vérifions quels disques sont disponibles :

```bash
lsblk
```

Ou avec :

```bash
fdisk -l
```

Si vous ajoutez un nouveau disque, assurez-vous qu'il est bien détecté (ex. `/dev/sda`).
![Capture d'écran 2025-02-19 151509](https://github.com/user-attachments/assets/ec901e4d-6b19-4442-b85f-45914702937f)

---

## 2. Installer LVM

Si LVM n'est pas encore installé, exécutez :

```bash
sudo apt update && sudo apt install lvm2 -y
```

Activez ensuite le module LVM :

```bash
sudo modprobe dm_mod
```

---

## 3. Créer le groupe de volumes LVM

### 3.1 Initialiser le disque pour LVM

Si le disque `/dev/sdb` est vierge, initialisez-le avec LVM :

```bash
sudo pvcreate /dev/sdb
```

Vérifiez l’état :

```bash
sudo pvs
```

### 3.2 Créer un groupe de volumes

Nommez le groupe `vg_data` :

```bash
sudo vgcreate vg_data /dev/sdb
```

Vérifiez :

```bash
sudo vgs
```

### 3.3 Créer un volume logique

Exemple : créer un volume de 50 Go nommé `lv_data` :

```bash
sudo lvcreate -L 50G -n lv_data vg_data
```

Vérifiez :

```bash
sudo lvs
```

---

## 4. Formater et monter le volume

### 4.1 Formater en ext4

```bash
sudo mkfs.ext4 /dev/vg_data/lv_data
```

### 4.2 Créer un point de montage et monter le volume

```bash
sudo mkdir -p /mnt/data
sudo mount /dev/vg_data/lv_data /mnt/data
```

Vérifiez le montage :

```bash
df -h
```

### 4.3 Rendre le montage permanent

Ajoutez cette ligne à `/etc/fstab` :

```bash
/dev/vg_data/lv_data /mnt/data ext4 defaults 0 2
```

Appliquez les changements :

```bash
sudo mount -a
```

---

## 5. Étendre un volume LVM (si nécessaire)

### 5.1 Agrandir le volume logique

Ajoutons 20 Go au volume logique :

```bash
sudo lvextend -L +20G /dev/vg_data/lv_data
```

### 5.2 Redimensionner le système de fichiers

Si vous utilisez `ext4` :

```bash
sudo resize2fs /dev/vg_data/lv_data
```

Si vous utilisez `XFS` :

```bash
sudo xfs_growfs /mnt/data
```

---

## 6. Vérification finale

Vérifiez l'état du stockage LVM :

```bash
sudo lvdisplay
sudo vgdisplay
sudo pvdisplay
```

Votre serveur Debian est maintenant configuré avec LVM pour une gestion avancée du stockage ! 🚀



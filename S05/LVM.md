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

Si le disque `/dev/sda` est vierge, initialisez-le avec LVM :

```bash
sudo pvcreate /dev/sda
```


Vérifiez l’état :

```bash
sudo pvs
```
![Capture d'écran 2025-02-19 153905](https://github.com/user-attachments/assets/4a1230d2-525d-4255-a92c-83d69d2c792c)
### 3.2 Créer un groupe de volumes

Nommez le groupe `vg_data` :

```bash
sudo vgcreate vg_data /dev/sda
```
![Capture d'écran 2025-02-19 155207](https://github.com/user-attachments/assets/8fa04604-a159-4438-af86-44be52372f32)

Vérifiez :

```bash
sudo vgs
```
![Capture d'écran 2025-02-19 155229](https://github.com/user-attachments/assets/7baebf47-a4e4-4235-9083-fbec1619d690)

### 3.3 Créer un volume logique

Exemple : créer un volume de 30 Go nommé `lv_data` :

```bash
sudo lvcreate -L 30G -n lv_data vg_data
```
![Capture d'écran 2025-02-19 155455](https://github.com/user-attachments/assets/1ee6f1cb-9fe1-41cc-b431-eaaf9078daeb)

Vérifiez :

```bash
sudo lvs
```
![Capture d'écran 2025-02-19 155603](https://github.com/user-attachments/assets/ac3427bf-d6f3-4403-9ccd-45d3bb82800d)

---

## 4. Formater et monter le volume

### 4.1 Formater en ext4

```bash
sudo mkfs.ext4 /dev/vg_data/lv_data
```
![Capture d'écran 2025-02-19 155819](https://github.com/user-attachments/assets/ddf1dc02-be81-4972-bd89-62644d938123)

### 4.2 Créer un point de montage et monter le volume

```bash
sudo mkdir -p /mnt/data
sudo mount /dev/vg_data/lv_data /mnt/data
```
![Capture d'écran 2025-02-19 155934](https://github.com/user-attachments/assets/44bd74eb-295a-4a23-b990-f848e910587a)

Vérifiez le montage :

```bash
df -h
```
![Capture d'écran 2025-02-19 160002](https://github.com/user-attachments/assets/5a7edc58-5730-4a59-9b57-76110a377386)


### 4.3 Rendre le montage permanent

Ajoutez cette ligne à `/etc/fstab` :

![Capture d'écran 2025-02-19 161808](https://github.com/user-attachments/assets/89a4f411-55d6-4d82-be94-4f464eac2120)

```bash
/dev/vg_data/lv_data /mnt/data ext4 defaults 0 2
```
![Capture d'écran 2025-02-19 160231](https://github.com/user-attachments/assets/4ffd7efd-1215-4512-8679-223fa0495f24)

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


sudo lvdisplay

![Capture d'écran 2025-02-19 160315](https://github.com/user-attachments/assets/3df31ce8-d50e-4569-86fc-7e3f0fa25060)

sudo vgdisplay

![Capture d'écran 2025-02-19 160338](https://github.com/user-attachments/assets/9942dee6-896f-449d-b8db-f48ccf067dc8)

sudo pvdisplay

![Capture d'écran 2025-02-19 160357](https://github.com/user-attachments/assets/f45b647b-c06a-4eef-b6da-dec036d24341)



Votre serveur Debian est maintenant configuré avec LVM pour une gestion avancée du stockage ! 🚀



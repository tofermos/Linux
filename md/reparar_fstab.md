# Reparar fstab des del Live

SiSi tens un error en el fitxer **`/etc/fstab`** que impedeix arrencar el sistema, segueix aquests passos per solucionar-ho des d'un entorn Live CD:

1. **Arrenca des del Live CD.**

2. **Identifica la partició del sistema:**
   Utilitza el comandament **`lsblk`** per veure les particions i localitzar la partició on tens el sistema instal·lat.

   ```bash
   lsblk
   ```

3. **Munta la partició del sistema:**
   Munta la partició del sistema en un directori, per exemple **`/mnt`**.

   ```bash
   sudo mount /dev/sdXn /mnt  # Substitueix /dev/sdXn per la partició correcta
   ```

4. **Remunta la partició en mode lectura-escriptura:**
   Si la partició es munta en mode només lectura, remunta-la en mode lectura-escriptura:

   ```bash
   sudo mount -o remount,rw /mnt
   ```

5. **Edita el fitxer `fstab`:**
   Modifica el fitxer **`/mnt/etc/fstab`** amb un editor de text (per exemple **nano**):

   ```bash
   sudo nano /mnt/etc/fstab
   ```

   Corregeix els errors que hi hagi (per exemple, rutes incorrectes, UUIDs incorrectes, opcions errònies).

6. **Desa i tanca l'editor.**

7. **Desmunta la partició:**
   Un cop hagis acabat, desmunta la partició per seguretat:

   ```bash
   sudo umount /mnt
   ```

8. **Reinicia el sistema:**
   Reinicia el sistema per veure si el problema s'ha solucionat.

   ```bash
   sudo reboot
   ```

---

Amb aquests passos hauries de poder corregir el fitxer **`fstab`** i permetre que el sistema arranqui correctament.

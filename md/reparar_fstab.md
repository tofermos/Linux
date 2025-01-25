# Reparar fstab des del Live

Si tens un error en el fitxer **`/etc/fstab`** que impedeix arrancar el sistema, segueix aquests passos per solucionar-ho des d'un entorn Live CD:

1. **Arrenca des del Live CD.**

(configurar el boot order...)

3. **Identifica la partició del sistema:**
   Utilitza el comandament **`lsblk`** per veure les particions i localitzar la partició on tens el sistema instal·lat.

   ```bash
   lsblk
   ```
![](png/lsblk.png)

4. **Munta la partició del sistema:**
   Munta la partició del sistema en un directori, per exemple **`/mnt`**.

   ```bash
   sudo mount /dev/sdXn /mnt  # Substitueix /dev/sdXn per la partició correcta
   ```
Al cas de l'exemple seria
 ```bash
   sudo mount /dev/sda1 /mnt  
   ```
5. **Remunta la partició en mode lectura-escriptura:**
   Si la partició es munta en mode només lectura, remunta-la en mode lectura-escriptura:

   ```bash
   sudo mount -o remount,rw /mnt
   ```

6. **Edita el fitxer `fstab`:**
   Modifica el fitxer **`/mnt/etc/fstab`** amb un editor de text (per exemple **nano**):

   ```bash
   sudo nano /mnt/etc/fstab
   ```

   Corregeix els errors que trobes (per exemple, rutes incorrectes, UUIDs incorrectes, opcions errònies) 
   o elimina les darreres modificacions que hages fet
   

8. **Tanca i alça els canvis**

9. **Desmunta la partició:**
   Una vegada acabes, desmunta la partició per seguretat:

   ```bash
   sudo umount /mnt
   ```

10. **Reinicia el sistema:**

(Revisa el boot order, treu el Live...)
Reinicia el sistema per veure si el problema s'ha solucionat.

   ```bash
   sudo reboot
   ```


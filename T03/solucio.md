# Solució: Seguretat Lògica - Recuperant Accés a Sistemes

## Part 1: Recuperació d'Accés

### 1. Afegir el disc de la tasca

Aquí tenim el disc:

![Imatge del disc de la tasca](./img/imatge_disk.png)

Afegim el disc de la tasca a la màquina virtual.

![Configuració del disc a VirtualBox](./img/imatge_1.png)

---

### 2. Canviar l'ordre d'execució

Cambiem l'ordre d'execució per assegurar que arrenca des del disc correcte.

![Ordre d'arrencada del sistema](./img/image_3.png)

---

### 3. Accedir al menú GRUB

Després d'iniciar la màquina, la reiniciem (des de dins).

![Reinici del sistema](./img/imatge_reniciar.png)

Seleccionem **opcions avançades**.

![Menú GRUB amb opcions avançades](./img/image_5.png)

---

### 4. Mode de recuperació

Seleccionem l'**opció de recuperació**.

![Selecció del mode recovery](./img/image_6.png)

---

### 5. Accedir com a root

En aquest part seleccionem **root** i fem **Control + D**.

![Menú de recuperació i accés root](./img/imatge_7.png)

---

### 6. Canviar la contrasenya

Ara canviem la contrasenya i reiniciem el sistema.

![Comanda passwd per canviar contrasenya](./img/imatge_8.png)

Així és com es reinicia la contrasenya.

![Verificació del canvi de contrasenya](./img/imatge_9.png)

---

## Part 2: Fortificació del GRUB

### 7. Generar hash de la contrasenya

Això és la comanda per transformar la teva contrasenya utilitzant hash:

![Comanda grub-mkpasswd-pbkdf2](./img/imatge_11.png)

---

### 8. Editar el fitxer GRUB

Obrim el fitxer GRUB.

![Edició del fitxer 40_custom](./img/image_12.png)

El guardem a "salida.txt".

![Contingut del fitxer amb el hash](./img/image_13.png)

---

### 9. Configurar usuari i hash

Copies el hash.

![Hash generat copiat](./img/image_14.png)

Poses el nom d'usuari desitjat i el hash al fitxer de configuració.

![Configuració amb superusers i password_pbkdf2](./img/imatge_17.png)

---

### 10. Actualitzar GRUB

Actualitzes el grub i fas un reboot.

![Comanda update-grub](./img/image_15.png)

---

### 11. Verificació final

Introdueixes:
- El teu nom d'usuari
- Contrasenya

![Pantalla de login del GRUB protegit](./img/image_16.png)

I ja està, això és la tasca.

![Accés correcte al sistema](./img/image_10.png)

---

## Conclusions

✅ S'ha recuperat l'accés al sistema canviant la contrasenya

✅ S'ha fortificat el GRUB amb protecció per contrasenya

✅ El sistema ara està protegit contra accessos no autoritzats

---

[⬅️ Tornar a la pàgina de la Tasca 3](./README.md) | [🏠 Tornar al repositori principal](../README.md)

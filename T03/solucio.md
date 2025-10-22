# Solució: Seguretat Lògica - Recuperant Accés a Sistemes

## Introducció

Un client ha perdut l'accés al seu portàtil amb **Zorin OS** (distribució Linux). La nostra tasca és recuperar l'accés al sistema i posteriorment fortificar-lo per evitar que aquest procediment es pugui repetir en cas de robatori.

---

## Part 1: Recuperació d'Accés al Sistema

### 1. Creació de la Màquina Virtual

Primer pas: configurar la màquina virtual amb el disc clonat del client.

![Configuració de la màquina virtual amb Zorin OS](./img/config-vm.png)

**Configuració utilitzada:**
- **Nom:** T03 Zorin
- **Sistema operatiu:** Linux - Ubuntu (64-bit)
- **Disc virtual:** El proporcionat pel client
- **Emmagatzematge:** Controlador SATA amb el disc .vdi

*Figura 1: Pantalla de configuració de VirtualBox amb el disc virtual del client*

---

### 2. Accés al Menú de Recuperació (GRUB)

En arrencar la màquina virtual, cal accedir al menú GRUB per poder entrar en mode de recuperació.

![Menú GRUB amb opcions d'arrencada](./img/grub-menu.png)

**Procediment:**
1. Reiniciar la màquina virtual
2. Prémer **Shift** o **Esc** durant l'arrencada
3. Apareix el menú GRUB amb les opcions avançades

**Opcions visibles:**
- Zorin
- Advanced options for Zorin
- Memory test (memtest86+)
- Memory test (memtest86+, serial console)

*Figura 2: Menú GRUB mostrant les opcions d'arrencada del sistema*

---

### 3. Selecció del Mode de Recuperació

Dins de les opcions avançades, seleccionem el mode de recuperació.

![Opcions avançades amb mode recovery](./img/recovery-mode.png)

**Opcions de recuperació:**
- Zorin, with Linux 6.x.x-xx-generic
- Zorin, with Linux 6.x.x-xx-generic (recovery mode)

Seleccionem l'opció **(recovery mode)** i premem **Enter**.

*Figura 3: Menú d'opcions avançades amb el mode de recuperació destacat*

---

### 4. Menú de Recuperació del Sistema

Un cop dins del mode de recuperació, apareix un menú amb diverses opcions.

![Menú de recuperació amb opcions disponibles](./img/recovery-options.png)

**Opcions disponibles:**
- resume (Reprendre l'arrencada normal)
- clean (Alliberar espai en disc)
- dpkg (Reparar paquets trencats)
- fsck (Revisar tots els sistemes d'arxius)
- grub (Actualitzar el carregador d'arrencada)
- network (Habilitar xarxa)
- root (Accedir a una shell amb privilegis de root)
- system-summary (Resum del sistema)

**Seleccionem:** `root` per accedir a una consola amb privilegis d'administrador.

*Figura 4: Pantalla del menú de recuperació del sistema Zorin OS*

---

### 5. Identificació de l'Usuari del Sistema

Des de la consola de root, cal identificar quin és l'usuari existent al sistema.

![Terminal mostrant la identificació de l'usuari Miquel Valls](./img/identificar-usuari.png)

**Comandes utilitzades:**
```bash
# Llistar usuaris del sistema
cat /etc/passwd | grep /home

# O veure directament les carpetes d'usuari
ls /home
```

**Usuari identificat:** `Miquel Valls` (nom d'usuari: `miquelvalls` o similar)

*Figura 5: Comandes per identificar els usuaris del sistema i resultat obtingut*

---

### 6. Canvi de Contrasenya de l'Usuari

Ara procedim a canviar la contrasenya de l'usuari identificat.

![Procés de canvi de contrasenya amb la comanda passwd](./img/canvi-contrasenya.png)

**Comanda utilitzada:**
```bash
passwd miquelvalls
```

**Procés:**
1. S'introdueix la nova contrasenya
2. Es confirma la contrasenya
3. El sistema confirma: "password updated successfully"

**Nova contrasenya establerta:** `felix` (per a l'exemple)

*Figura 6: Execució de la comanda passwd i confirmació de canvi correcte*

---

### 7. Verificació de l'Accés

Després de reiniciar el sistema en mode normal, comprovem que podem accedir.

![Pantalla d'inici de sessió amb l'usuari i la nova contrasenya](./img/verificacio-acces.png)

**Resultat:** ✅ Accés correcte amb la nova contrasenya.

L'usuari **Miquel Valls** ja pot accedir al sistema i recuperar la documentació important.

*Figura 7: Pantalla d'inici de sessió mostrant accés exitós amb la nova contrasenya*

---

## Part 2: Fortificació del Sistema

### 8. Investigació: Protecció del GRUB

Després de recuperar l'accés, el client demana **protegir el sistema** per evitar que es pugui repetir aquest procediment.

**Solució:** Protegir el GRUB amb contrasenya per evitar l'accés al mode de recuperació sense autorització.

#### Fonts d'informació consultades:
- [Recuperant Password en Linux](https://waytoit.wordpress.com/2013/06/06/recuperando-password-en-ubuntu/)
- Documentació oficial de GRUB: https://www.gnu.org/software/grub/manual/
- Ubuntu Documentation: https://help.ubuntu.com/community/Grub2/Passwords

---

### 9. Generació del Hash de Contrasenya

Per protegir el GRUB, necessitem crear un hash segur de la contrasenya.

![Generació del hash de contrasenya amb grub-mkpasswd-pbkdf2](./img/generar-hash.png)

**Comanda utilitzada:**
```bash
grub-mkpasswd-pbkdf2
```

**Procés:**
1. S'introdueix la contrasenya desitjada
2. Es confirma la contrasenya
3. El sistema genera un hash encriptat (PBKDF2)

**Hash generat:**
```
grub.pbkdf2.sha512.10000.[llarg_hash_encriptat]
```

*Figura 8: Terminal mostrant el procés de generació del hash per protegir GRUB*

---

### 10. Configuració del Fitxer GRUB

Ara cal editar els fitxers de configuració de GRUB per afegir la protecció.

![Edició del fitxer de configuració de GRUB](./img/editar-grub.png)

**Fitxer a editar:** `/etc/grub.d/40_custom`

**Comanda:**
```bash
sudo nano /etc/grub.d/40_custom
```

**Contingut afegit:**
```bash
set superusers="admin"
password_pbkdf2 admin grub.pbkdf2.sha512.10000.[hash_generat]
```

**Explicació:**
- `set superusers="admin"`: Defineix l'usuari administrador del GRUB
- `password_pbkdf2 admin [hash]`: Assigna el hash de contrasenya a l'usuari

*Figura 9: Editor nano mostrant el fitxer 40_custom amb la configuració de seguretat*

---

### 11. Actualització de la Configuració GRUB

Després de modificar els fitxers, cal actualitzar la configuració del GRUB.

![Execució de la comanda update-grub](./img/update-grub.png)

**Comanda:**
```bash
sudo update-grub
```

**Resultat:**
El sistema regenera els fitxers de configuració del GRUB incorporant les noves mesures de seguretat.

*Figura 10: Terminal mostrant l'actualització correcta de la configuració GRUB*

---

### 12. Verificació de la Protecció

Reiniciem el sistema i comprovem que ara el GRUB demana contrasenya.

![GRUB protegit demanant credencials abans d'accedir a opcions avançades](./img/grub-protegit.png)

**Comprovacions:**
1. ✅ El menú GRUB ja no permet editar opcions sense contrasenya
2. ✅ Per accedir al mode de recuperació cal introduir usuari i contrasenya
3. ✅ L'arrencada normal del sistema continua funcionant sense demanar contrasenya

**Usuari:** `admin`  
**Contrasenya:** La definida durant la generació del hash

*Figura 11: Menú GRUB sol·licitant autenticació per accedir a opcions avançades*

---

## Conclusions

### Objectius aconseguits

✅ **Recuperació d'accés:** S'ha accedit correctament al sistema i s'ha restablert la contrasenya de l'usuari Miquel Valls.

✅ **Identificació de vulnerabilitats:** S'ha demostrat que un sistema Linux sense protecció al GRUB és vulnerable a recuperació de contrasenyes.

✅ **Fortificació del sistema:** S'ha implementat protecció per contrasenya al GRUB utilitzant hash PBKDF2.

✅ **Documentació completa:** Tot el procés ha quedat documentat amb captures per a futures referències.

### Recomanacions finals per al client

1. **Mantenir actualitzat:** Assegurar-se que el sistema operatiu i GRUB estiguin sempre actualitzats.

2. **Contrasenya forta al GRUB:** Utilitzar una contrasenya diferent i robusta per al GRUB.

3. **Encriptació del disc:** Per a màxima seguretat, considerar l'encriptació completa del disc (LUKS).

4. **Còpia de seguretat:** Fer còpies de seguretat regulars de les dades crítiques.

5. **BIOS/UEFI protegida:** També protegir amb contrasenya la BIOS/UEFI del portàtil.

### Seguretat aconseguida

Amb aquestes mesures, en cas de robatori del portàtil:
- ❌ No es pot accedir al mode de recuperació sense contrasenya
- ❌ No es poden editar les opcions d'arrencada del GRUB
- ✅ Les dades queden protegides davant accessos no autoritzats físics

---

## Referències

- Recuperant Password en Linux: https://waytoit.wordpress.com/2013/06/06/recuperando-password-en-ubuntu/
- GRUB 2 Manual: https://www.gnu.org/software/grub/manual/
- Ubuntu GRUB2 Passwords: https://help.ubuntu.com/community/Grub2/Passwords
- Apunts RA1AA4 Seguretat Lògica

---

[⬅️ Tornar a la pàgina de la Tasca 3](./README.md) | [🏠 Tornar al repositori principal](../README.md)

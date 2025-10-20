# Tasca 3 - Seguretat Lògica: Recuperant Accés a Sistemes

## 📋 Descripció del Projecte

Un client ha arribat a la consultora amb un **portàtil amb Zorin OS** (distribució Linux amb entorn gràfic) que utilitzava un directiu. El problema: **ha oblidat la contrasenya** i cal recuperar l'accés per obtenir documentació crítica.

Per evitar danys a l'equip original, s'ha clonat el disc en un **disc virtual** per treballar-hi de forma segura.

## 🎯 Objectius de la Tasca

### Part 1: Recuperació d'accés
1. Crear una màquina virtual i connectar el disc virtual
2. Accedir al sistema vulnerant el GRUB
3. Identificar l'usuari existent
4. Assignar una contrasenya nova
5. Verificar l'accés correcte

### Part 2: Fortificació del sistema
6. Investigar mètodes per protegir l'accés al GRUB
7. Configurar protecció per contrasenya al GRUB
8. Evitar que es pugui repetir el procediment de recuperació

## ⚠️ Context de Seguretat

Després de la recuperació, el client demana **fortificar el sistema** per evitar que, en cas de robatori del portàtil, algú pugui accedir a la informació utilitzant el mateix procediment.

## 📄 Contingut d'aquesta carpeta

- **[solucio.md](./solucio.md)** - Documentació completa del procediment amb captures
- **img/** - Captures de pantalla de tot el procés

## 🔗 Accés Directe

➡️ **[VEURE LA SOLUCIÓ COMPLETA](./solucio.md)**

---

## 📚 Material de Referència

- Disc virtual proporcionat pel client
- Apunts RA1AA4 Seguretat Lògica
- [Recuperant Password en Linux](https://waytoit.wordpress.com/2013/06/06/recuperando-password-en-ubuntu/)
- Documentació oficial de GRUB

## 🛠️ Eines Utilitzades

- VirtualBox / VMware
- Zorin OS (Linux)
- Editor de configuració GRUB
- Mode de recuperació de Linux

---

[⬅️ Tornar al repositori principal](../README.md)

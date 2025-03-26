### 🚀 Accès et Ressources  

- **Présentation** : [PowerPoint](https://docs.google.com/presentation/d/14e_twIQS-2SKcZEQ-ROKGhTkDDycoEc3QtUY7cwCmz4/)  
- **Allocations** : [Nos ressources](https://ccdb.alliancecan.ca/me/allocations)  
- **Documentation** : [Docs techniques](https://docs.alliancecan.ca/wiki/Technical_documentation/fr)  

---

### 📊 Optimisation des ressources

- **Optimisation CPU/GPU** : [Optimisation des ressources](Optimisation%20des%20ressources.md)

---

### 🖥️ Clusters et Configuration  

- **Chaque nœud** : Linux | **64 cœurs max** | **4 To RAM max** | **1-28 jours** | **8 GPU max**  
- **Remplacements** :
  - **Rorqual** → Remplace **Béluga**
  - **Narval** → Reste actif
  - **Nibi** → Remplace **Graham**
  - **Fir** → Remplace **Cedar**  
- **Choisir un cluster proche** (ex: **Narval** à l’ETS)  
- **Consulter la doc** pour connaître le matériel disponible  

---

### 🔑 Connexion  

1. **Connexion SSH**  
   ```bash
   ssh user@server.alliancecan.ca
   ```
   _Exemple_ :  
   ```bash
   ssh myname@narval.alliancecan.ca
   ```

2. **Double authentification (obligatoire)**  
   - Installer **Duo Mobile** : [Guide d'installation](https://docs.alliancecan.ca/wiki/Multifactor_authentication/fr#Utiliser_un_téléphone_ou_une_tablette)  

---

### 📂 Transfert et Stockage  

1. **Transfert de fichiers**  
   - **SCP** :  
     ```bash
     scp [source] [destination]
     ```
     _Exemple_ :  
     ```bash
     scp file1.dat username@grappe:dir1/
     ```
   - **Gros fichiers** : Utiliser **Globus**  

2. **Stockage (max 40 To toutes grappes confondues)**  
   - **Home (50 Go, 500k fichiers)** : Code/scripts  
   - **Scratch (20 To, 1M fichiers)** : Temporaire (effacé après 2 mois)  
   - **Project (1 To, 500k fichiers)** : Données volumineuses/partagées
   - **Nearline (1 To, 5000 fichiers)** : Archives  
   - **$SLURM_TMPDIR (960 Go)** : Stockage rapide (effacé après calcul)  

3. **Vérifier l’espace disque**  
   ```bash
   diskusage_report
   ```

---

### 🛠️ Installation de logiciels  

1. **Modules disponibles**  
   - **Rechercher un module**  
     ```bash
     module spider [mot_clé]
     ```
   - **Vérifier la disponibilité**  
     ```bash
     module avail [module]
     ```
   - **Charger un module**  
     ```bash
     module load [module]
     ```
   - **Décharger un module**  
     ```bash
     module unload [module]
     ```
   - **Lister les modules chargés**  
     ```bash
     module list
     ```
   - **Réinitialiser l’environnement**  
     ```bash
     module purge
     ```

   📌 **Liste complète** : [Modules disponibles](https://docs.alliancecan.ca/wiki/Available_software/fr)  

2. **Python & Wheels**  
   - **Environnement 2023** → Python **3.10 - 3.13**  
   - **Lister les wheels** :  
     ```bash
     avail_wheels
     avail_wheels *torch*
     ```
   📌 **Détails** : [Python Wheels](https://docs.alliancecan.ca/wiki/Available_Python_wheels/fr)  

   🚨 **Ne pas utiliser Conda sur une grappe** → Utiliser `pip install`  

3. **Exemple : Environnement Python**  
   ```bash
   module load python/3.8
   virtualenv env --no-download
   source env/bin/activate
   pip install --no-index seaborn==0.13
   deactivate
   ```

   **Répliquer un environnement** :  
   ```bash
   pip freeze > requirements.txt
   pip install -r requirements.txt
   ```

---

### 🏗️ Exécution de tâches  

📌 **Toutes les tâches doivent être soumises via l’ordonnanceur (PAS sur les nœuds de connexion)**  

1. **Exemple de script SLURM (`script.sh`)**  
   ```bash
   #!/bin/bash
   #SBATCH --account=def-sponsor00
   #SBATCH --nodes=1
   #SBATCH --cpus-per-task=1
   #SBATCH --mem-per-cpu=256M
   #SBATCH --time=0:01:00

   module load python/3.12
   python --version
   ```

2. **Soumettre une tâche**  
   ```bash
   sbatch script.sh
   ```

3. **Voir l’état des tâches**  
   ```bash
   squeue -u $USER
   ```

4. **Annuler une tâche**  
   ```bash
   scancel <jobid>
   ```

5. **Statistiques d’une tâche terminée**  
   ```bash
   seff <jobid>
   ```

---

💡 **Exemple d’exécution complète**  
```bash
sbatch script.sh
seff <jobid>
```

---

### ❗ En cas de problème  

🔍 Vérifier les fichiers de sortie  
🔍 Consulter le [Wiki](https://docs.alliancecan.ca/wiki/Technical_documentation/fr)
🔍 Contacter le support : 📧 **support@tech.alliancecan.ca**  

---

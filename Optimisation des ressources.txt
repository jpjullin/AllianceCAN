# ⚙️ Optimisation CPU/GPU — Calcul Québec

## 🚀 Présentation
- **Présentation** : [PowerPoint](https://docs.google.com/presentation/d/1tS6q2KERVvBToZFm4_9leqUagaYb36qte5mPQLuXojc)

---

## 🎯 Objectif

- **Optimiser l’utilisation des ressources** CPU / GPU  
- **Éviter le gaspillage** : des ressources demandées mais non utilisées sont **perdues**  
- **Surveiller l’efficacité des tâches** et leur adéquation aux ressources demandées  

---

## 📊 Système de Priorisation

- **Ordonnancement via SLURM**  
- La **priorité est partagée** entre les membres d’un même projet  
- **Bonnes pratiques influencent la priorité** :  
  - ✅ **Demander moins** que la cible → **priorité augmente**  
  - ❌ **Dépasser** la cible → **priorité diminue**  
- La **priorité se régénère automatiquement** avec une **demi-vie d’une semaine**  

📌 **Vérifier la priorité actuelle** :  
```bash
sshare -l -A def-prof -u prof1,grad2,postdoc3
```
- Valeur > 1 : priorité élevée  
- Valeur < 1 : priorité faible  

---

## 🧠 Stratégies de Soumission

- **Segmenter les calculs** sur différentes grappes (Narval, Béluga…)  
- **Vérifier les portails** :  
  - [Narval](https://portail.narval.calculquebec.ca)  
  - [Béluga](https://portail.beluga.calculquebec.ca)  
  - [Documentation des portails](https://docs.alliancecan.ca/wiki/Portail)  

💡 Une tâche finie **plus tôt** ne coûte que le temps utilisé, mais **réduit la priorité**  

---

## 🧵 Processus vs Fils d’exécution

- **Processus** : Instance autonome d’un programme  
- **Fils (threads)** : Unités d’exécution au sein d’un processus (partagent code, mémoire, variables) 

---

## 🖥️ Paramétrage des Tâches

- `--cpus-per-task=1` → Exécution **sérielle**  
- Tâches **multithread** :  
  - Plusieurs **threads** dans un **même** processus  
  - **Mémoire partagée**  
  - **Tous les CPUs** doivent être utilisés  
  - 🧠 **20 % de mémoire en trop** : OK. Au-delà → à ajuster  

- Tâches **multiprocesseur** :  
  - Plusieurs **processus** parallèles  
  - **Mémoire distribuée**, non partagée  
  - Utiliser `srun` pour lancer la tâche (ex : MPI)  
  - `--ntasks=3` → 3 processus, possiblement répartis sur plusieurs nœuds  

💡 Les **nœuds à grande mémoire** sont rares → temps d’attente potentiellement long  

🔴 `--mem=0` → Demande **toute la mémoire du nœud**  

---

## 🧠 Adapter les Ressources

- **Vérifier la doc des librairies utilisées** pour :  
  - Threading ?  
  - Besoins mémoire ?  
  - Utilisation CPU/GPU ?  

---

## 🧪 GPU — Bonnes Pratiques

- **Narval** : 4 GPU A100 par nœud (40 Go chacun)  
- **SM Active** : > 80 %  
- **SM Occupancy** : > 50 %  
- **Tensor Utilisation** : aussi haut que possible  

📊 Vérifier l’utilisation sur le portail de la grappe  

---

## 🧩 GPU : Profils & MIG

- **MIG (Multi-Instance GPU)** :  
  - Fractionnement d’un GPU en sous-unités indépendantes  
  - ✅ Temps d’attente réduit  
  - ➕ Idéal pour des tâches GPU > 10 %  

📌 Exemple de profil MIG :  
```bash
--gres=gpu:a100_3g.20g:1
```
→ GPU A100, profil 3g.20g (20 Go)  

🔧 Profils disponibles :  
- `1g.5gb`, `2g.10gb`, `3g.20gb`, `4g.20gb`

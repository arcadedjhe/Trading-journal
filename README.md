# Trading-journal

# Trading Journal PWA - Version 2.1.0 🎯

**Date de sortie** : 02 avril 2026

---

## 🎉 NOUVEAUTÉS v2.1.0 - MONEY MANAGEMENT

### 📊 **Onglet Money Management complet**
- **Dashboard temps réel** (mode Real uniquement)
- **Circuit Breakers** : Surveillance DD Daily/Weekly/Monthly
- **Alertes visuelles** : États colorés avec barres de progression
- **Badge alertes** : Notification rouge sur l'onglet MM si alertes critiques

### 📈 **5 nouveaux graphiques MM**
1. **Expectancy glissante** (30 derniers trades)
2. **Profit Factor glissant** (50 derniers trades)
3. **Drawdown Intraday** (cumul par jour)
4. **Séries de pertes maximales** (évolution)
5. **Distribution R** (histogramme)

### ⚙️ **Configuration avancée**
- **Seuils personnalisables** dans Config → Money Management
- **Circuit Breakers** : Daily (-2.5%), Weekly (-5%), Monthly (-8%), Global (-10%)
- **Invalidation stratégie** : Expectancy, PF, WR, pertes consécutives
- **Tracking par stratégie** : Métriques séparées pour chaque stratégie active

### 🔧 **Améliorations**
- **Duplication Real→Paper** : Note auto "📝 Trade passé en réel"
- **Graphiques temporels** : P&L par Mois et par Semaine (onglet Analyse)
- **Palette harmonisée** : Couleurs cohérentes (vert émeraude / rouge corail)

---

## 📦 DÉPLOIEMENT GITHUB PAGES

### **Fichiers à uploader**
```
trading-journal/
├── index.html (renommer trading-journal-indexeddb.html)
├── service-worker.js
├── manifest.json
├── icon-192.png (existant)
└── icon-512.png (existant)
```

### **Étapes**
1. **Renommer** `trading-journal-indexeddb.html` → `index.html`
2. **Upload** sur GitHub Pages (branche `main` ou `gh-pages`)
3. **Vérifier** que l'URL fonctionne : `https://arcadedjhe.github.io/trading-journal/`
4. **Tester** l'onglet MM en mode Real

---

## 🧪 TESTS À EFFECTUER

### **Tests critiques**
- [ ] Passage mode Paper → Real : onglet MM apparaît
- [ ] Passage mode Real → Paper : onglet MM masqué
- [ ] Sauvegarder paramètres MM dans Config
- [ ] Créer des trades et vérifier dashboard MM
- [ ] Badge alertes rouge si DD proche seuil
- [ ] 5 graphiques s'affichent correctement

### **Tests fonctionnels**
- [ ] Circuit Breakers : statuts colorés corrects
- [ ] Expectancy glissante : calcul sur 30 trades
- [ ] PF glissant : calcul sur 50 trades
- [ ] DD Intraday : cumul par jour
- [ ] Distribution R : bins de 0.5R
- [ ] Tracking par stratégie : métriques séparées

---

## 🎯 UTILISATION ONGLET MM

### **Accès**
1. Passer en mode **Real** (toggle en haut)
2. Cliquer sur onglet **📊 MM**
3. Dashboard affiche état temps réel

### **Dashboard**
- **Circuit Breakers** : DD Daily/Weekly/Monthly avec barres de progression
- **État par Stratégie** : Expectancy 30T + série pertes actuelle
- **Alertes Actives** : Liste rouge (critical) ou jaune (warning)

### **Graphiques**
- **Expectancy** : Tendance sur 30 derniers trades (ligne)
- **PF** : Évolution sur 50 derniers trades (ligne)
- **DD Intraday** : Performance quotidienne (barres)
- **Loss Streaks** : Séries maximales tous les 10 trades (barres)
- **Distribution R** : Répartition des résultats (histogramme)

### **Badge alertes**
- **Badge rouge** : Alertes critiques détectées
- **Pas de badge** : Tout est OK

---

## ⚙️ CONFIGURATION MM

### **Accès**
Config → Money Management (avant section Information)

### **Paramètres**
**Circuit Breakers :**
- Daily DD (%) : -2.5 par défaut
- Weekly DD (%) : -5 par défaut
- Monthly DD (%) : -8 par défaut
- Global Multi-Strat (%) : -10 par défaut

**Seuils Invalidation :**
- Expectancy Alerte/Invalid (R) : 0.1 / 0
- PF Alerte/Invalid : 1.2 / 1.0
- WR Alerte/Invalid (%) : 28 / 25
- Pertes Consécutives Alerte/Invalid : 10 / 15

**Sauvegarde :**
- Cliquer "💾 Sauvegarder les paramètres MM"
- Paramètres stockés séparément Paper/Real dans IndexedDB

---

## 🔄 RESET DRAWDOWN

### **Automatique**
- **Daily DD** : Reset à minuit (00:00)
- **Weekly DD** : Reset le lundi (00:00)
- **Monthly DD** : Reset le 1er du mois (00:00)

### **Calcul**
```javascript
DD% = (P&L période / Capital) × 100
```

---

## 📝 ARCHITECTURE TECHNIQUE

### **Nouvelles fonctions**
```javascript
// Chargement/Sauvegarde paramètres
loadMMSettings()          → Retourne settings depuis IndexedDB
saveMMSettings()          → Sauvegarde settings dans IndexedDB

// Calculs Drawdown
calculateDailyDrawdown()   → DD% depuis minuit
calculateWeeklyDrawdown()  → DD% depuis lundi
calculateMonthlyDrawdown() → DD% depuis 1er mois

// Métriques glissantes
calculateRollingExpectancy(n, strategy)    → Expectancy sur n trades
calculateRollingProfitFactor(n, strategy)  → PF sur n trades
calculateRollingWinRate(n, strategy)       → WR% sur n trades
getCurrentLossStreak(strategy)             → Série pertes actuelle

// Alertes
checkMoneyManagementAlerts() → Array d'alertes {level, type, message}

// UI
updateMMDashboard()  → Affiche dashboard complet
updateMMCharts()     → Affiche 5 graphiques
updateMMBadge()      → Met à jour badge alertes
```

### **Intégrations**
- `init()` : Charge paramètres MM au démarrage
- `saveTrade()` : Appelle updateMMBadge() si mode Real
- `deleteTrade()` : Appelle updateMMBadge() si mode Real
- `switchTab('mm')` : Appelle updateMMDashboard()
- `toggleFeesField()` : Masque/affiche onglet MM selon mode

### **Stockage IndexedDB**
```
config store:
├── mmSettings_paper  → Paramètres MM mode Paper
└── mmSettings_real   → Paramètres MM mode Real
```

---

## 🐛 BUGS CORRIGÉS

- `loadMMSettings()` : Retourne maintenant defaults si rien en DB
- `deleteTrade()` : await saveTrades() ajouté pour persistance
- Boutons onglets : Reset styles correct dans updateUITheme()

---

## 📊 COMPATIBILITÉ

- **Navigateurs** : Chrome, Edge, Firefox, Safari (dernières versions)
- **Mobile** : iOS Safari, Chrome Android
- **PWA** : Installable sur mobile et desktop
- **Offline** : Fonctionne hors ligne (Service Worker)

---

## 🚀 PROCHAINES ÉVOLUTIONS POSSIBLES

- **Graphiques MM supplémentaires** : Equity curve, sharpe ratio
- **Export MM** : Rapport PDF Money Management
- **Alertes email/push** : Notifications externes si seuils atteints
- **Multi-timeframe** : Analyse MM sur différentes périodes
- **Comparaison stratégies** : Graphiques côte à côte

---

## 📞 SUPPORT

En cas de problème :
1. Vérifier la console navigateur (F12)
2. Vérifier que mode Real est actif
3. Vérifier qu'il y a des trades complétés
4. Recharger la page (Ctrl+R)
5. Vider le cache si nécessaire (Ctrl+Shift+R)

---

**Version** : 2.1.0  
**Date** : 02/04/2026  
**Développeur** : Jerome  
**URL** : https://arcadedjhe.github.io/trading-journal/

---

**🎯 L'onglet Money Management est un outil professionnel pour suivre et protéger ton capital en temps réel !**

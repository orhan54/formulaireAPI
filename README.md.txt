# API Formulaire - Codes d'Absence AFPA

API JSON pour gérer les codes d'absence des stagiaires AFPA.

## Installation

```bash
git clone https://github.com/orhan54/formulaireAPI.git
cd formulaireAPI
npm install
npm start
```

## Codes disponibles

- **Code 01** : Retard absence non rémunéré
- **Code 04** : Grève Afpa
- **Code 05** : Maladie
- **Code 06** : Absence Légale (mariage, naissance, décès...)
- **Code 20-41** : Accidents (travail, trajet, hors Afpa)
- **Code 98** : Absence autorisée non rémunérée (Pôle Emploi, recherche emploi...)
- **Code 99** : Absence non autorisée non rémunérée

## Utilisation

### Récupérer tous les codes

```javascript
async function getCodes() {
  const response = await fetch('http://localhost:3000/api/codes');
  const data = await response.json();
  return data;
}
```

### Récupérer un code spécifique

```javascript
async function getCode(codeId) {
  const response = await fetch(`http://localhost:3000/api/codes/${codeId}`);
  const data = await response.json();
  return data;
}

// Exemple
const code06 = await getCode('code06');
// Retourne: { title: "Code 06 : Absence Légale", option: [...] }
```

### Soumettre une absence

```javascript
async function declareAbsence(absence) {
  const response = await fetch('http://localhost:3000/api/absences', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(absence)
  });
  return await response.json();
}

// Exemple
await declareAbsence({
  codeId: 'code05',
  option: 'Maladie',
  stagiaire: {
    nom: 'Dupont',
    prenom: 'Jean',
    matricule: '12345'
  },
  dateDebut: '2024-12-18',
  dateFin: '2024-12-20'
});
```

## Structure JSON

```json
{
  "code06": {
    "title": "Code 06 : Absence Légale",
    "option": [
      "Naissance ou adoption (3 jours)",
      "Mariage (4 jours)",
      "Mariage d'un enfant (1 jour)",
      "Décès d'un conjoint ou du partenaire lié par un pacte civil de solidarité ou d'un enfant (2 jours)",
      "Décès du père, de la mère, du beau-père, de la belle-mère, d'un frère ou d'une soeur (1 jours)",
      "Visite médicale obligatoire de la grossesse",
      "Stagiaire élu membre du CA d'une caisse de sécurité sociale"
    ]
  }
}
```

## Licence

MIT - [@orhan54](https://github.com/orhan54)
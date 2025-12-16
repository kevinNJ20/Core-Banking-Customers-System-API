# Core Banking Customers System API

API System qui expose les opérations CRUD sur les clients Core Banking via Supabase.

## Description

Cette API fournit un accès standardisé aux données clients du système Core Banking. Elle suit le pattern API-Led Connectivity comme System API dans la couche d'intégration.

## Endpoints

### GET /api/customers
Récupère la liste des clients avec filtres optionnels.

**Query Parameters:**
- `customerNumber` (optional): Numéro de client
- `status` (optional): Statut du client (Active, Inactive, etc.)
- `lastName` (optional): Nom de famille (recherche partielle)
- `taxId` (optional): Identifiant fiscal
- `limit` (optional, default: 100): Nombre de résultats
- `offset` (optional, default: 0): Offset pour pagination

### POST /api/customers
Crée un nouveau client.

**Body:** Customer object (voir RAML pour la structure)

### GET /api/customers/{customerId}
Récupère un client par son ID.

### PUT /api/customers/{customerId}
Met à jour un client existant.

**Body:** Customer object (voir RAML pour la structure)

### POST /api/customers/search
Recherche avancée de clients.

**Body:** SearchCriteria object

## Configuration

### Base de Données Supabase

1. Exécuter le script SQL d'initialisation dans Supabase
2. Configurer les propriétés dans `src/main/resources/config.properties`:

```properties
# Supabase Database Configuration
supabase.db.url=jdbc:postgresql://your-project.supabase.co:5432/postgres
supabase.db.user=your-user
supabase.db.password=your-password

# HTTP Configuration
http.port=8081
http.host=0.0.0.0
```

### Tables Utilisées

- `customers`: Table principale des clients
- Relation avec les données de partie (first_name, last_name, etc.)

## Démarrage

### Prérequis

- Java 8 ou supérieur
- Maven 3.6+
- Accès à Supabase (base de données PostgreSQL)
- Mule Runtime 4.x (ou Anypoint Studio)

### Compilation

```bash
mvn clean package
```

### Exécution Locale

```bash
mvn mule:run
```

L'API sera disponible sur: `http://localhost:8081/api/*`

### Déploiement CloudHub

```bash
mvn deploy -DmuleDeploy
```

## Architecture Technique

### Pattern APIkit Router

Cette API utilise **APIkit Router** avec un seul listener HTTP dans `interface.xml` qui route les requêtes vers les flows business-logic appropriés.

**Structure:**
- `interface.xml`: Contient le listener HTTP principal et le router APIkit
- `core-banking-customers-system-api.xml`: Contient les flows business-logic

### Flows Business-Logic

- `get-customers-business-logic`: Logique de récupération de liste
- `create-customer-business-logic`: Logique de création
- `get-customer-by-id-business-logic`: Logique de récupération par ID
- `update-customer-business-logic`: Logique de mise à jour
- `search-customers-business-logic`: Logique de recherche

## Exemples de Requêtes

### GET /api/customers

```bash
curl -X GET "http://localhost:8081/api/customers?limit=10&offset=0"
```

### POST /api/customers

```bash
curl -X POST http://localhost:8081/api/customers \
  -H "Content-Type: application/json" \
  -d '{
    "customerNumber": "CUST001",
    "party": {
      "partyType": "Individual",
      "firstName": "John",
      "lastName": "Doe",
      "dateOfBirth": "1990-01-15",
      "taxId": "123-45-6789"
    },
    "status": "Active",
    "customerSegment": "Retail"
  }'
```

### GET /api/customers/{customerId}

```bash
curl -X GET "http://localhost:8081/api/customers/123e4567-e89b-12d3-a456-426614174000"
```

### PUT /api/customers/{customerId}

```bash
curl -X PUT http://localhost:8081/api/customers/123e4567-e89b-12d3-a456-426614174000 \
  -H "Content-Type: application/json" \
  -d '{
    "status": "Inactive",
    "customerSegment": "Premium"
  }'
```

## Bonnes Pratiques Mule 4

✅ **APIkit Router**: Utilisation d'un seul listener HTTP avec routing automatique  
✅ **Séparation des concerns**: Interface dans `interface.xml`, logique métier dans `core-banking-customers-system-api.xml`  
✅ **Error Handling**: Gestion d'erreurs standardisée avec try/catch  
✅ **DataWeave 2.0**: Transformations modernes et performantes  
✅ **Property Placeholders**: Configuration externalisée via `config.properties`

## Dépendances

Voir `pom.xml` pour la liste complète. Principales dépendances:

- Mule HTTP Connector
- Mule Database Connector
- PostgreSQL JDBC Driver
- Mule APIkit Module
- DataWeave Module

## Tests

Les tests MUnit sont dans `src/test/munit/`.

Pour exécuter les tests:

```bash
mvn test
```

## Documentation API

La spécification RAML complète est disponible dans:
- `src/main/resources/api/core-banking-customers-system-api.raml`

## Support

Pour toute question, consultez la documentation principale dans le README.md à la racine du projet.


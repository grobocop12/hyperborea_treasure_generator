
# Dokument kontekstowy: Generator Skarbów do Hyperborei 3E  
(Wersja rozszerzona o C4, ERD i README)

---

## 1. Cel systemu
Celem systemu jest umożliwienie graczom oraz Mistrzom Gry wygodnego generowania skarbów do gry Hyperborea 3E na podstawie tabel skarbów opisanych w zewnętrznych plikach konfiguracyjnych.

---

## 2. Użytkownicy systemu
- **Użytkownik anonimowy** – generuje skarby, brak historii  
- **Użytkownik zalogowany** – posiada historię wygenerowanych skarbów  
- **Administrator** – zarządzanie użytkownikami i tabelami

---

## 3. Zakres
**W zakresie**
- generowanie skarbów  
- odczyt tabel  
- zapis wyników  
- autoryzacja  
- panel admina  

**Poza zakresem**
- edycja tabel przez UI  
- złożone wieloetapowe losowania  

---

## 4. Kontekst techniczny
- **Frontend:** Lite TS/JS + Vite + Preact  
- **Backend:** Java + Spring Boot  
- **Baza danych:** PostgreSQL  

---

## 5. Przepływ działania
1. Użytkownik wysyła żądanie losowania  
2. Backend pobiera tabelę i losuje wynik  
3. Wynik wraca do użytkownika  
4. Wynik zapisuje się w historii (dla zalogowanych)

---

## 6. Modele danych

### User
- id  
- username  
- email  
- password_hash  
- role  
- created_at  

### TreasureTable
- id  
- name  
- file_path  
- version  
- uploaded_at  

### GeneratedTreasure
- id  
- user_id  
- table_id  
- result_json  
- generated_at  

---

## 7. C4 – Diagram System Context (tekstowy)

```
[Anon User] → [Frontend Preact] ↔ [Backend Spring Boot] ↔ [Pliki JSON/YAML]
                     ↓
               [Zalogowany User]
                     ↓
                [PostgreSQL DB]
```

---

## 8. C4 – Container (tekstowy)

```
+------------------------------------------------------------+
|                    Generator Skarbów                       |
+------------------------------------------------------------+
|  Frontend (Preact + Vite)                                  |
|  Backend (Spring Boot)                                     |
|  PostgreSQL (Users, Tables, Treasures)                     |
+------------------------------------------------------------+
```

---

## 9. ERD – Diagram tekstowy

```
users (id PK, username, email, password_hash, role, created_at)
    |
    | FK user_id
    v
generated_treasures (id PK, user_id, table_id, result_json, generated_at)
    ^
    | FK table_id
    |
treasure_tables (id PK, name, file_path, version, uploaded_at)
```

---

## 10. README (draft)

# Generator Skarbów – Hyperborea 3E

## Opis
Aplikacja webowa do generowania skarbów na podstawie tabel z Hyperborei 3E.

## Technologie
- Preact + Vite  
- Spring Boot  
- PostgreSQL  

## Uruchamianie

### Backend
```
./mvnw spring-boot:run
```

### Frontend
```
npm install
npm run dev
```

## Struktura repo
- /frontend – Preact  
- /backend – Spring Boot  
- /tables – JSON/YAML  

## API (skrót)
- POST /auth/login  
- POST /auth/register  
- GET /treasures/generate  
- GET /history  

# Flight Booking System - Distribuirani Sistem za Rezervaciju Letova

## Arhitektura Sistema

Sistem se sastoji od **5 komponenti**:

```
┌─────────────┐         ┌──────────────┐         ┌─────────────┐
│   Client    │────────▶│    Server    │────────▶│    DB 1     │
│   (React)   │         │   (Flask)    │         │   (MySQL)   │
│  Port 5173  │◀────────│  Port 5000   │         │             │
└─────────────┘         └──────────────┘         └─────────────┘
                               │                         
                               │ Redis Cache             
                               │                         
                               ▼                         
                        ┌──────────────┐         ┌─────────────┐
                        │Flight Service│────────▶│    DB 2     │
                        │   (Flask)    │         │   (MySQL)   │
                        │  Port 5001   │         │             │
                        └──────────────┘         └─────────────┘
```

## Komponente

### 1. Client (React + Vite)
- Frontend aplikacija
- Komunikacija: REST API + WebSocket
- Port: 5173

### 2. Server (Flask)
- Glavni backend servis
- Autentifikacija, korisnici, avio kompanije
- Komunikacija sa Redis kešom
- Port: 5000

### 3. Database 1 (MySQL)
- Podaci o korisnicima
- Autentifikacioni podaci
- Avio kompanije

### 4. Flight Service (Flask)
- Mikroservis za letove
- Asinhrona obrada kupovine karata
- Port: 5001

### 5. Database 2 (MySQL)
- Podaci o letovima
- Kupljene karte
- Ocene letova

## Funkcionalnosti

### Autentifikacija
- JWT token autentifikacija
- 3 neuspešna pokušaja = blokada na 15 minuta
- Hešovane lozinke (bcrypt)

### Uloge
- **KORISNIK**: Kupuje karte, ocenjuje letove
- **MENADŽER**: Kreira letove i avio kompanije
- **ADMINISTRATOR**: Odobrava letove, generiše izvještaje, menja uloge

### Letovi
- 3 taba: Predstojeci, U toku (sa tajmerom), Završeni/Otkazani
- Real-time obaveštenje admina o novom letu (WebSocket)
- Asinhrona kupovina karata
- Ocenjivanje letova (1-5)

### Email Notifikacije
- Promena uloge korisnika
- Otkazivanje leta
- PDF izvještaji

### Generisanje Izvještaja
- PDF izvještaji za sve tabove
- Slanje na email

## Tehnologije

### Backend
- Python 3.11+
- Flask
- SQLAlchemy (ORM)
- Redis (Keš)
- JWT
- Bcrypt
- Flask-SocketIO (WebSocket)
- Multiprocessing (Procesi)

### Frontend
- React 18+
- Vite
- Axios
- Socket.IO Client
- React Router

### Database
- MySQL 8.0+

### Deployment
- Docker
- Docker Compose

## Struktura Projekta

```
flight-booking-system/
├── client/                 # React aplikacija
├── server/                 # Flask backend
├── flight-service/         # Flight mikroservis
├── docker-compose.yml      # Docker orchestration
└── README.md
```

## Bodovanje (Max 90 poena)

1. Flask aplikacija + Baza: **51 poen**
2. React/Angular UI: **10 poena**
3. Redis keš: **14 poena**
4. Procesi: **10 poena**
5. Docker: **10 poena**
6. Više računara: **5 poena**

**UKUPNO: 100 * 0.9 = 90 poena**

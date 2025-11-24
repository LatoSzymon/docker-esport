# Docker-Esport

A containerized platform setup for e-sports-related services. This project demonstrates how to use Docker and Docker Compose to run multiple services related to an esports ecosystem in a scalable and isolated environment.

Key Components & Architecture:
- Multi-container architecture using docker-compose, enabling easy orchestration of different services.
- Backend service(s): containerized backend logic (e.g., REST API or game-management backend).
- Database service: separate container for persistent storage (e.g., MongoDB / SQL) depending on your setup.
- Scalability & Isolation: each service runs in its own container, making local development, testing, and scaling easier.
- Configuration via environment variables: all essential settings (ports, credentials) are configurable via .env or compose files.

-------------------------------------------------------------------------------------------------------------------------------------------------

(PL) Projekt na zaliczenie przedmiotu technologie chmurowe, połączony z projektem z bezpieczeństwa aplikacji web


Celem było stworzenie pełnej aplikacji webowej, uruchamianej przez dockera i/lub kubernetesa. Mamy tu prostą obsługę i zarządzanie bazą esportowych graczy i drużyn oraz zabezpieczenie jej.

Przed użyciem zapoznaj się z treścią instrukcji dołączonej do repozytorium w pliku README, bądź skonsultuj się z lekarzem lub kompetentnym programistą, gdyż każdy kod niewłaściwie stosowany zagraża twojemu życiu lub poczuciu estetyki.

INSTRUKCJA URUCHOMIENIA:
(komendy uruchamiane w głównym katalogu projektu)

1. Upewnij się że kubernetes jest włączony (kubectl musi działać)
2. Jeśli jeszcze nie masz, zainstaluj ingressa 

    kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.9.5/deploy/static/provider/cloud/deploy.yaml

3. Zbuduj obrazy dockerowe:

    docker build -t frontend:latest ./frontend
    docker build -t data-service:latest ./data-service
    docker build -t analytics-service:latest ./analytics-service

4. utworzenie configmap dla realm z keycloaka
 kubectl create configmap keycloak-realm --from-file=realm-export.json=./k8s/keycloak/realm-export.json
5. Wdrożenie wszystkich komponentów

    kubectl apply -f k8s/mongo/
    kubectl apply -f k8s/data-service/
    kubectl apply -f k8s/analytic-service/
    kubectl apply -f k8s/frontend/
    kubectl apply -f k8s/keycloak/
    kubectl apply -f k8s/ingress.yaml

6. Upewnienie się, że wszystkie pody uruchomiły się bez błędów (kubectl get pods)
7. przejdź pod http://localhost/ , keycloak poprosi o zalogowanie,
8. zaloguj się jako któryś z użytkowników:
    - user (hasło: user) - zwykły użytkownik bez specjalnych uprawnień
    - admin (hasło: admin) - jak sama nazwa wskazuje, jest to admin

9. Opis działania.
    Przy pierwszym uruchomieniu baza graczy i drużyn jest pusta (wolumenów z danymi nie ma w repo), ale z pozycji admina można swobodnie dodawać/edytować/usuwać graczy i drużyny. Zwykły użytkownik takich możliwości, jak można się domyślić, nie ma





Wszystkie zadania wykonaj za pomocą Docker CLI.
• Zadanie 1: Sprawdź czy w Twoim lokalnym rejestrze Docker są obrazy, za pomocą polecenia: docker images
Jeżeli tak, usuń wszystkie za pomocą polecenia docker rmi <nazwa_obrazu>
• Zadanie 2: Uruchom kontener Nginx za pomocą polecenia: docker run –t –d nginx /bin/bash
Sprawdź jaki jest jego status za pomocą docker ps –a i odczytaj Container_ID.
• Zadanie 3: Za pomocą polecenia docker exec –t -i <Container_ID> /bin/bash podłącz się do działającego kontenera.
Wykonaj polecenie bash: ls
Następnie wyjdź z kontenera: exit
• Zadanie 4: Za pomocą polecenia: docker stop <Container_ID> zatrzymaj wcześniej uruchomiony kontener.
• Link do dokumentacji Docker CLI:
https://docs.docker.com/engine/reference/commandline/cli/

Link do wykonanego zadania: https://docs.google.com/document/d/1b4tOIJuheUsdei4VC5owzGjr2a49zvikbnnIzNZ7mBg/edit?usp=sharing

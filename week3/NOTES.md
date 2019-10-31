`docker run -it -p 8081:80 nginx /bin/bash`

`-it` - kontener zostanie uruchomiony w trybie interaktywnym

Wywołana na końcu komenda `/bin/bash` zostanie wywołana przy uruchomieniu kontenera. Jeżeli w obrazie mamy zdefiniowany `ENTRYPOINT` to nam to nie zadziała ponieważ `Docker` podczas startu kontenera łączy ze sobą podaną komendę z ewentualną komendą w `ENTRYPOINT`, aby to obejść należy wówczas
użyć flagi `--entrypoint /bin/bash` przy uruchomieniu.

---

`docker commit` - pozwala nam stworzyć obraz z kontenera (kontener może być zatrzymany).

> Raczej traktować jako ciekawostkę. Obrazy tworzymy najczęściej przez Dockerfile

`docker commit <container> <image-name>`

`docker commit 2d new-img`

---

`docker history <image>` - pozwala prześledzić historię zmian w danym obrazie

---

`docker cp` - pozwala skopiować pliki za równa z kontenera do hosta jak i z hosta do kontenera

`docker cp 'plik-z-hosta' '<container>:sciezka do pliku w-kontenerze'

`docker cp 'hello-from-host.txt' '123123:hello-from-container.txt'

---

`docker exec` - pozwala uruchomić komendę w działającym kontenerze

np

`docker exec -it 98 /bin/bash`

---

`docker stats` - pozwala nam na żywo zobaczyć jakie statystyki generuje nasz kontener jeśli chodzi o zużycie CPU/RAM/IO

---

`docker logs` - pozwala a wyświetlić nam logi kontenera

`docker logs <container> -f` - pozwala śledzić aktualnie pojawiające się komunikaty

---

`docker inspect` - pozwala zobaczyć bardzo szczegółowe informacje nt naszych obrazów czy kontenerów.

Polecenie to pozwoli zobaczyć m.in. pełną konfigurację sieciową dla kontenera, porty które zostały dla niego ustawione,
aktualny status czy informację o niepowodzeniu przy uruchomieniu. `Docker inspect` jest podstawowym rozwiązaniem jeżeli chcemy
zdiagnozować dlaczego nasz kontener nie działa lub zachowuje się niezgodnie z naszymi oczekiwaniami.

---

`docker diff` - pozwala nam sprawdzić jakie zaszły zmiany w kontenerze w systemie plików po jego uruchomieniu

---

`docker tag` - pozwala zmienić nazwę obrazu w lokalnym repozytorium.

---

`docker pull` - pozwala nam pobrać obraz bez jego uruchamiania

---

`docker build` - pozwala wygenerować obraz bazując na pliku Dockerfile

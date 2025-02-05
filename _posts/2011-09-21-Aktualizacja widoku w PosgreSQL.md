---
title: Aktualizacja widoku w PosgreSQL
date: 2011-09-21 09:12:54 +0100
categories: [database]
tags: [database,postgresql,view]
image:
  path: /assets/img/postgresql.png
  alt: widoki w postgresql
author: michal_cwiklinski
toc: false
---

# Aktualizacja widoku w PosgreSQL

Pracując na bazie danych PostgreSQL natchnąłem się na informacje dotyczące wykonywania update'ów na widokach. Oczywiście dotyczy to wersji >9.0 tego silnika baz danych, ale - po kolei.

Ogarnimy moją sytuację. Mam dwie tabele: message i messagetext.
```sql
CREATE TABLE message (
    id serial PRIMARY KEY,
    sender integer NOT NULL,
    recipient integer NOT NULL,
    date timestamp without timezone NOT NULL,
    messagetextid integer NOT NULL
);
CREATE TABLE messagetext (
    textid serial PRIMARY KEY,
    subject character varying NOT NULL,
    message text NOT NULL
);
```

Jedna przetrzymuje instancje wiadomości (nadawca, odbiorca, daty, itp), a druga teksty (oczywiście w grę wchodzą wiadomości "do wielu"). Aby pobrać listę wiadomości, trzeba albo łączyć te dwie tabele, albo joinować. I tu z pomocą przychodzi widok.
```sql
CREATE VIEW messages AS
    SELECT message.id, message.recipient, message.date, message.sender, messagetext.textid, messagetext.message, messagetext.subject
        FROM message
        JOIN messagetext ON message.messagetextid = messagetext.textid;
```

Teraz pobranie listy to po prostu:
```sql
SELECT * FROM messages
```

Domyślnie wszystkie widoki zachowują się jak tabele, ale w trybie tylko do odczytu. Próba wykonania na nich aktualizacji zakończy się błędem, ponieważ silnik bazodanowy nie będzie wiedział, jak zaktualizować fizyczne tabele kryjące się pod definicją widoku:
```sql
Błąd SQL:
ERROR:  cannot update a view
HINT:  You need an unconditional ON UPDATE DO INSTEAD rule.
```

W poleceniu:
```sql
UPDATE messages SET sender=3 WHERE id=3
```

Z pomocą w tym przypadku przychodzą nam reguły. Widoki modyfikowalne tworzone są dzięki systemowi tych reguł.
```sql
CREATE RULE messages_upd AS
    ON UPDATE TO messages DO INSTEAD (
        UPDATE message SET sender = NEW.sender, recipient=NEW.recipient, date=NEW.date WHERE id = NEW.id;
        UPDATE messagetext SET subject=NEW.subject, message=NEW.message WHERE textid = NEW.textid;
    );
```
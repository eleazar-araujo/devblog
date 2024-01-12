---
layout: [post, post-xml]              # Pflichtfeld. Nicht ändern!
title:  "Untersuchung gängiger Cache-Eviction Räumungsstrategien: LRU, TLRU, LFU, and MRU"         # Pflichtfeld. Bitte einen Titel für den Blog Post angeben.
date:   2024-01-12 09:00              # Pflichtfeld. Format "YYYY-MM-DD HH:MM". Muss für Veröffentlichung in der Vergangenheit liegen. (Für Preview egal)
modified_date: 2024-01-12 09:00             # Optional. Muss angegeben werden, wenn eine bestehende Datei geändert wird.
author_ids: [earaujo]       # Pflichtfeld. Es muss in der "authors.yml" einen Eintrag mit diesen Namen geben.
categories: [Softwareentwicklung]     # Pflichtfeld. Maximal eine der angegebenen Kategorien verwenden.
tags: [Microservices, Cache, Performance]   # Bitte auf Großschreibung achten.
---
Caching ist eine grundlegende Technik, die eingesetzt wird, um die Systemleistung zu verbessern, indem häufig aufgerufene Daten in einem schnelleren, aber begrenzten Speicherbereich gespeichert werden. 
Ein kritischer Aspekt des Caching ist die Verwaltung dieses begrenzten Speicherplatzes, und "Cache-Eviction Policies" spielen eine entscheidende Rolle bei der Entscheidung, welche Elemente entfernt werden sollen, wenn der Cache voll ist. 
In diesem Artikel werden wir vier gängige Cache-Eviction näher angehen: Least Recently Used (LRU), Time-aware LRU (TLRU), Least Frequently Used (LFU) und Most Recently Used (MRU).

# Least Recently Used (LRU)

LRU ist eine weit verbreitete Cache-Eviction Policy, die auf dem Prinzip basiert, dass die Elemente, auf die zuletzt zugegriffen wurde, die wahrscheinlichsten Kandidaten für eine Räumung sind. 
Dieses Verfahren eignet sich besonders für Anwendungen, bei denen der jüngste Zugriff ein starker Indikator für die zukünftige Nutzung ist. 
Ein Beispiel ist der Cache eines Webbrowsers, in dem kürzlich besuchte Seiten mit größerer Wahrscheinlichkeit wieder aufgerufen werden als ältere Seiten.

## Vorteile

- Einfach und intuitiv.
- Oft effektiv bei der Erfassung der zeitlichen Lokalität, d.h. wenn kürzlich verwendete Elemente wahrscheinlich bald wieder verwendet werden.

## Nachteile

- Erfordert das Pflegen und Aktualisieren von Zeitstempeln für jedes Element.
- Könnte bei Szenarien, in denen sich die Zugriffsmuster schnell ändern, Probleme bereiten.

# Time-aware LRU (TLRU)

TLRU ist eine Erweiterung von LRU, die einen Zeitfaktor in die Auslagerungsentscheidung einbezieht. 
Es erkennt an, dass sich die Relevanz von Elementen im Laufe der Zeit ändern kann und daher die Häufigkeit des Zugriffs nicht der einzige Faktor für die Räumung sein muss. 
TLRU könnte sich für Anwendungen mit Daten eignen, die eine zeitabhängige Relevanz aufweisen. 
In einem Nachrichtenaggregationsdienst kann TLRU beispielsweise eingesetzt werden, um aktuellen Nachrichtenartikeln unter Berücksichtigung ihrer Popularität in einem bestimmten Zeitfenster Priorität einzuräumen.

## Vorteile

- Behebt die Beschränkung von reinem LRU in dynamischen Systemen.
- Ermöglicht eine feiner abgestimmte Räumungsstrategie.

## Benachteiligungen

- Erhöht die Komplexität im Vergleich zum einfachen LRU.
- Erfordert eine sorgfältige Abstimmung der Zeitparameter.

# Least Frequently Used (LFU):
LFU ist eine Cache-Eviction Policy, die sich auf die Häufigkeit des Zugriffs konzentriert. 
Diese Richtlinie eignet sich gut für Szenarien, in denen auf bestimmte Elemente konstant häufiger zugegriffen wird als auf andere. 
In einem Datenbanksystem könnte LFU für die Zwischenspeicherung von Abfrageergebnissen effektiv sein, wenn beliebte Abfragen wahrscheinlich häufiger ausgeführt werden. 
LFU passt sich gut an unterschiedliche Zugriffsmuster an und kann in Situationen, in denen einige Elemente konstant beliebt sind, eine bessere Leistung bieten.

## Vorteile

- Wirksam in Szenarien, in denen auf bestimmte Elemente konstant häufiger zugegriffen wird als auf andere.
- Kann sich gut an unterschiedliche Zugriffsmuster anpassen.

## Nachteile

- Komplexität bei der Verwaltung der Zugriffshäufigkeitszähler.
- Kann Probleme bereiten, wenn die Zugriffsmuster unvorhersehbar oder unbeständig sind.


# Zuletzt verwendet (MRU)
MRU ist das Gegenteil von LRU, da es die Elemente, auf die zuletzt zugegriffen wurde, entfernt, wenn der Cache sein Limit erreicht hat. 
Diese Richtlinie eignet sich für Anwendungen, bei denen der jüngste Zugriff ein starker Prädiktor für die künftige Nutzung ist und bei denen es unwahrscheinlich ist, dass ältere Elemente erneut aufgerufen werden. 
Ein Beispiel hierfür ist die Echtzeit-Analyse, bei der die jüngsten Datenpunkte für die Analyse relevanter sind als ältere Daten.

## Vorteile

- Einfachheit der Implementierung.
- Geeignet für Szenarien, in denen der jüngste Zugriff ein besserer Indikator für die künftige Nutzung ist.

## Nachteile

- Kann in Szenarien mit zyklischen Zugriffsmustern nicht gut funktionieren.
- In manchen Fällen können Elemente vorzeitig entfernt werden.

# Fazit

Die Wahl der richtigen Cache-Eviction Policy hängt von den spezifischen Merkmalen der Anwendung und den Zugriffsmustern der Daten ab. 
Jede Richtlinie hat ihre Stärken und Schwächen, und die Auswahl der am besten geeigneten Richtlinie erfordert eine sorgfältige Analyse der Systemanforderungen. 
Da sich die Systeme weiterentwickeln und sich die Zugriffsmuster ändern, ist es wichtig, die Cache-Eviction Policies regelmäßig zu überprüfen und gegebenenfalls anzupassen, um eine optimale Leistung zu gewährleisten.

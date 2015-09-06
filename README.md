#Dokumentation

##Inhaltsverzeichnis
  * [Einleitung] (https://github.com/Mr-Kabelbruch/WBA2SS15SinghSchmiedebergMathesdorf/blob/master/Dokumentation.md#einleitung)
  * [Dokumentation des Service] (https://github.com/Mr-Kabelbruch/WBA2SS15SinghSchmiedebergMathesdorf/blob/master/Dokumentation.md#dokumentation-des-service)
   * Angabe der Ressourcen, der dafür vorgesehenen http Verben und deren Semantik
   * Überlegungen die angestellt wurden zur Definition der Ressourcen, Alternativen die betrachtet wurden
   * Beschreibung der Anwendungslogik und der Datenhaltung mit Überlegungen dazu
   * Beschreibung der Funktionalität, die aus Zeitmangel nicht umgesetzt werden konnte
  * [Dokumentation des Dienstnutzers] (https://github.com/Mr-Kabelbruch/WBA2SS15SinghSchmiedebergMathesdorf/blob/master/Dokumentation.md#dokumentation-des-dienstnutzers)
   * Beschreibung der Anwendungslogik und der Datenhaltung mit Überlegungen dazu
   * Umsetzung der Präsentationslogik und Überlegungen dazu
   * ggfs. Beschreibung der asynchron implementierten Teile der Nutzungsschnittstelle und Begründungen dazu
   * Beschreibung der Funktionalität, die aus Zeitmangel nicht umgesetzt werden konnte
  * [Dokumentation des Prozesses] (https://github.com/Mr-Kabelbruch/WBA2SS15SinghSchmiedebergMathesdorf/blob/master/Dokumentation.md#dokumentation-des-prozesses)
   * Beschreibung der Vorgehensweise, auch der Irrwege
   * Eine kritische Reflexion des Erreichten und des Nicht-Erreichten in einem Fazit
   * Eine Arbeitsmatrix, aus der hervorgeht, wer im Team an welchen Aktivitäten zu welchem Grad (%) beteiligt war


####Einleitung
Im folgenden wird eine Webanwendung beschrieben, in dem Veranstalter eine Event (Name der Veranstaltung, Datum, Uhrzeit, Musikrichtung und Raumkapazitäten) bekannt machen kann und frei Dienstleister wie Caterer, Sicherheit und Techniker, usw. sich  für das event eintragen können.</br> 
Das wird über die webanwendung organisiert, die Vorteile sind es werden keine dritten Agenturen involviert und man hat am Ende eine Liste mit zuverlässigen Personal, die bei gelegenheit wieder für ein Veranstalter arbeiten kann. </br>

Ziel der Webanwendung ist es hauptsächlich organisationstechnische Fragen im vorraus zu klären und dem Veranstalter eine Liste mehrer qualifzierter Dienstleister zu erstellen und den Diestleister ein Plattform zu bieten, in dem sie die Veranstalter sofort ohne Umwege (Agenturen) anfragen können.

Jeder Nutzer hat sein eigenes Profil, dass ihn als einzelne Person, oder als Firma verkörpert. In dem Profil werden Obligatorische Daten wie Kontaktdaten und Rolle angezeigt, sowie Qualifikationen. Jeder Akteur kann sich über die Startseite, in sein Account einloggen. 

Das System soll zudem, über eine Suche verfügen und einem Bewertungsystem, damit jeder erkennen kann wer z.b zuverlässig, gut bezahlt oder gut organisiert ist.

</br>

####Dokumentation des Service:</br>
  * Angabe der Ressourcen, der dafür vorgesehenen http Verben und deren Semantik 

    Es wurden folgende zwei Objekte generalisiert User und das Event.</br>
    
    ######User sowohl veranstalter als auch Dienstleister
    
    | Ressourcen        | Jeweilige Verb | Ressourccen Pfad | Content_typ (req)      | Content_typ (res)  |
    | -------------     |:-------------: | -----------------|----------------------  |-----------------   | 
    |Profil erstellen   | mit einem Post | /user            | application/json       | empty              |
    |Profil ändern      | mit einem Put  | /user/:id        | application/json       | empty              |
    |Profil anzeigen    | mit einem Get  | /user/:id        | application/json       | application/json   |
    |Profil löschen     |mit einem Delete| /user/:id        | application/json       | empty              |
    |Alle Profil zeigen |mit einem Get   | /user            | application/json       | application/json   | 
    
    </br>
    
    ######Event welches nur von Veranstalter erstellt werden kann
    
    | Ressourcen       | Jeweilige Verb | Ressourccen Pfad                   |Content_typ (req)      | Content_typ (res)  |
    | -------------    |:-------------: | ---------------------------------  |---------------------- |-----------------   | 
    |Event erstellen   | mit einem Post | /user/:uid/event/                  |  application/json     | empty              |
    |Event ändern      | mit einem Put  | /user/:uid/event/:eid              |  application/json     | empty              |
    |Event löschen     |mit einem Delete| /user/:uid/event/:eid              |  application/json     | empty              |
    |Event anzeigen    |mit einem get   | /user/:uid/event/:eid              |  application/json     | application/json   |
    |alle Events zeigen|mit einem get   | /user/:uid/event                   |  application/json     | application/json   |
     
    ######Favorietenliste
    
    | Ressourcen       | Jeweilige Verb | Ressourccen Pfad                   |Content_typ (req)      | Content_typ (res)  |
    | -------------    |:-------------: | ---------------------------------  |---------------------- |-----------------   | 
    |fav hinzufügen    | mit einem Put  | /user/:uid/fav/:fid                |   application/json    | empty              |
    |fav   löschen     |mit einem Delete| /user/:uid/fav/:fid                |   application/json    | empty              |
    
    
    * Überlegungen die angestellt wurden zur Definition der Ressourcen, Alternativen die betrachtet wurden 
    </br>
    
    Es wurde festgestellt, dass ein Veranstalter und der Dienstleister jeweils Ressourcen sind, allerdings aus der
    realen Welt, die sich im System als Profile wieder finden. Aus den Profilen heraus werden die Rollen in  Veranstalter und
    Dienstleister raus selektiert, um die verschieden Authentifizierungen zu ermöglichen, die jede Rolle mit sich bringt z.b
    {"Rolle":"Veranstalter"} die in der Datenbank unter user hinterlegt wird. Nach der Rolle wird gefiltert, wenn z.b ein
    Veranstalter ein Event erstellen möchte, macht er dies über die Ressource /user/:uid/event/. Es muss vorher
    sichergestellt sein, dass der Veranstalter auch ein Veranstalter ist, weil nur ein Veranstalter hat die rechte dazu ein
    Event zu erstellen. Das gleiche gilt für das löschen von eines Event nur das die Ressource etwas anders aufgebaut sind
    user/:uid/event/:eid.</br>
    
    
    Daraus können wiederrum Subressourcen entstehen. Der Veranstalter Erstellt ein Event und in dem können sich die
    Dienstleister eintragen, somit ist das Event die Subressource des Veranstalters, aber auch des Dienstleisters den dieser
    kann sich in ein Event eintragen.</br> 
    
    Die Dienstleister können eine Listenressource erstellt in dem hervorgeht für welche Event sie sich eingetragen haben. 
    In dem man sich in ein Event einträgt wird eine Listenressource erstellt aus der man dann später alle z.b Köche die sich
    für das jeweilige Event eingetragen haben auflisten kann und somit auf eine Vielzahl qualifizierter Mitarbeiter zugreifen
    kann.</br>
    Die Überlegung war welche Entitäten werden benötigt um am Ende eine Liste mit guten Personal zu bekommen und generell
    eine Auflistung von allen Events.
    
    * Beschreibung der Anwendungslogik und der Datenhaltung mit Überlegungen dazu </br>
 
    Über die Anwendungslogik kann man Profile erstellen, abrufen,  verändern,  löschen und alle Profile abrufen, das wäre die
    Basis. Man kann unter anderem Event erstellen, abrufen,  verändern,  löschen und alle Events abrufen, allerdings können
    wie zuvor schon erwähnt nur die Veranstalter die ein Event erstellen und löschen. Der Dienstleister kann sich nur für das
    jeweilige Event ein- und austragen.  Des Weiteren soll der User,  in dem Fall Veranstalter und Dienstleister in der Lange
    sein eine Favoritenliste zu erstellen und z.b ein besonders guter Mitarbeiter wird darauf gesetzt, damit in Zukunft
    wieder mit diesem gearbeitet werden kann. Das gleiche Prinzip gilt für die Dienstleister die gute Veranstalter auf ihre
    Liste setzen können, so kann man ein  Netzwerk  nach und nach aufbauen.
    Die User bekommen eine Datensatz, in dem sowohl eine eindeutige id vergeben wird, als auch Vorname, Nachname und Rolle
    gespeichert wird  und man kann mit der jeweiligen rolle verschiedene Funktionen ausführen. 
    Das Event bekommt ein Datensatz mit den Inhalt: Name, Datum, Info.</br>
    
    Die oben genannten Datensätze sind obligatorisch. Es können optional auch noch andere Daten für die User verwendet werden
    wie Qualifikation, Ort, Erfahrung, Alter, usw. Das gleiche gilt auch für  das Event, da wären Optional Raumkapazität, Art
    der Veranstaltung, Musikrichtung, Ort.


  * Beschreibung der Funktionalität, die aus Zeitmangel nicht umgesetzt werden konnte </br>
  
    Die Favoritenliste,Suche, Bewertung, Nachrichten verschicken konnten leider nich 

####Dokumentation des Dienstnutzers:
  * Beschreibung der Anwendungslogik und der Datenhaltung mit Überlegungen dazu </br>
    In der Dienstnutzer Anwendung der capp.js kann man neue Benutzer ertellen und die 
  * Umsetzung der Präsentationslogik und Überlegungen dazu </br>
  * ggfs. Beschreibung der asynchron implementierten Teile der Nutzungsschnittstelle und Begründungen dazu </br>
  * Beschreibung der Funktionalität, die aus Zeitmangel nicht umgesetzt werden konnte </br>

####Dokumentation des Prozesses:
  * Beschreibung der Vorgehensweise, auch der Irrwege </br>
  * Eine kritische Reflexion des Erreichten und des Nicht-Erreichten in einem Fazit </br>
  * Eine Arbeitsmatrix, aus der hervorgeht, wer im Team an welchen Aktivitäten zu welchem Grad (%) beteiligt war </br>

 
|               | Carolina Singh    | Robin Mathesdorf|**gesamt**|
|---------------|------------       |---------------- |----------|
|Idee & Projekt | 33%               | 33%             | **100%** |
|Layout         | 100%              | 0%              | **100%** |
|Code           | 90%               | 10%             | **100%** |








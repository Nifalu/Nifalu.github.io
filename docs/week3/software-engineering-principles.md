---
layout: default
---

# Prinzipien des Software Engineerings


*Wir haben bereits die ersten drei von sieben wichtigen Grundprinzipien genauer angeschaut, nämlich* Teilen der Verantwortlichkeiten, Modularisierung *und* Abstraktion. 
*In diesem Artikel werden wir die weiteren Grundprinzipien genauer anschauen.*

## Genauigkeit und formales Vorgehen 

Softwareentwicklung ist ein kreativer Prozess. Trotzdem ist es unerlässlich, dass wir dabei ingenieurmässig, also systematisch, exakt und wenn möglich auch formal arbeiten. 
Nur durch dieses systematische und exakte Arbeiten wird es möglich, zuverlässig Software von hoher Qualität zu entwickeln. 
Exaktes und auch formales Arbeiten steht keineswegs im Kontrast zu kreativem Arbeiten. Im Gegenteil: Klare Regeln und klar definierte Komponenten können eine wichtige 
Voraussetzung für das Erschaffen von kreativen Werken sein. So würde niemand auf die Idee kommen, Johann Sebastian Bach als nicht kreativ zu bezeichnen, weil er sich 
an die starren Regeln der Musik und Harmonielehre gehalten hat. 

Es gibt ganz verschiedene Stufen von Exaktheit. Die höchste Form ist dabei das (mathematisch) formale Vorgehen. Obwohl im Bereich Softwareengineering viel Forschungsarbeit im Gebiet der formalen Verifikation geleistet wurde, werden die wenigsten Softwareingenieure in ihrer Karriere ein Programm komplett formal spezifizieren können. 
Genaues und systematisches Arbeiten ist aber trotzdem während dem gesamten Entwicklungsprozess gefragt. So ist es zum Beispiel wichtig, dass ein Programm rigoros getestet wird, oder, dass der Entwicklungsprozess exakt und systematisch dokumentiert wird. 

Ein Bereich, in dem sogar auch in der Praxis formal gearbeitet wird, ist die Spezifikation von Typsignaturen in Code.
Die Typsignaturen in folgendem Code geben uns bereits starke hinweise darauf, was die Funktion macht. 
```java
int foo(String bar) { ??? }
```
Wir wissen, es wird ein Argument vom Typ String entgegengenommen, und darauf wird eine Ganzzahl erzeugt. 
Beide diese Objekte, `int` und `String` sind genau formal definiert und wir wissen, welche Werte sie annehmen können und welche Operationen auf ihnen definiert sind. Dank solcher Typannotationen kann uns auch der Compiler unterstützen. Er überprüft anhand der Regeln der mathematischen Logik, dass unsere Verwendung der Funktionen im Programm aus Typsicht korrekt ist.

Moderne Typensysteme lassen uns hier auch immer komplexere Sachverhalte spezifizieren. Die folgende Typsignatur stammt aus der Java Standardbibliothek. Diese Typsignatur besagt, dass die Methode ```map```, auf 
einen Stream vom Typ ```Stream<T>``` angewendet, einen Stream vom Typ ```Stream<R>``` zurückgibt, wenn die Funktion ```mapper``` ein Argument von Typ ```T``` in einen Wert von Typ  ```R``` abbildet. 
```java 
interface Stream<T> extends BaseStream<T, Stream<T> > {
   <R> Stream<R> map(Function<? super T, ? extends R> mapper);
}
```
Die Forschung im Bereich der Typsysteme ist sehr aktiv. Es werden immer neue Typsysteme entwickelt, welche immer komplexere Sachverhalte spezifizieren können.

## Veränderungen voraussehen


Software ist andauernden Veränderungen ausgesetzt. Häufige Auslöser für Änderungen sind, dass Fehler behoben werden müssen, neue Anforderungen gestellt werden, oder sich die Umgebung (z.B. das Betriebssystem) ändert. Deshalb haben wir auch Wartbarkeit als eines der wichtigen Qualitätskriterien definiert.  

Die Fähigkeit von Software sich Veränderungen anpassen zu können passiert nicht aus Zufall. Zur Designzeit muss vorausgesehen werden, welche Aspekte einer Software sich wahrscheinlich ändern werden. Das Design kann dann so angepasst werden, dass diese Veränderungen einfach durchgeführt werden können. 

Ein gutes Beispiel für ein Design, welches Veränderungen vorausgesehen hat ist das Design von Java. 
Ein Designziel von Java war, das Java Programme portabel sind, also auf verschiedenen Plattformen laufen können. 
Es wurde vorausgesehen, dass sich aber Hardware und Betriebssystemfunktionalitäten laufend ändern würden und es somit mit grossen Aufwand verbunden wäre, die Java Umgebung laufend anzupassen. Die Lösung, die das Java Entwicklungsteam gewählt hat ist, dass Java Programme nicht direkt auf dem Betriebssystem laufen, sondern auf einer Virtuellen Maschine. Ändert sich nun die Funktionalität vom Betriebssystem, muss nur die Virtuelle Maschine angepasst werden. Die Java Software kann jedoch unverändert weiterlaufen. Damit muss nur ein kleiner, wohldefinierter Teil der Software angepasst werden, was viel weniger Aufwand bedeutet.
  

## Allgemeinheit

Das Prinzip der Allgemeinheit lässt sich wie folgt zusammenfassen:

*Wann immer ein spezifisches Problem gelöst werden muss, kann man sich fragen, ob es denn ein generelleres Problem gibt, welches sich hinter diesem Problem versteckt.*
Häufig ist das verallgemeinerte Problem gar nicht schwieriger zu lösen. Die Lösung zum verallgemeinerten Problem kann aber viel breiter angewendet werden.
Statt vieler spezifischer Lösungen, erhalten wir so oft eine Lösung, die viele ähnliche Probleme löst. Typische Beispiele von solchen allgemeinen Lösungen finden sich im Bereich von Algorithmen. Niemand würde heute noch einen eigenen Sortieralgorithmus schreiben. Stattdessen wird auf die Standardbibliothek zurückgegriffen, welche bereits einen Sortieralgorithmus implementiert hat. Dabei muss man dem Algorithmus nur noch mitteilen, welche Ordnungsrelation auf den Daten definiert wird. Alles andere ist bereits implementiert.

Das folgende, einfache Beispiel, veranschaulicht, wie man die obige ```map``` Funktion spezifisch für den Typ ```int```, als auch allgemein schreiben kann. 

Die spezifische Variante:
```java
    public interface IntList {
        List<String> map(Function<Integer, String> mapper) { 
            List<String> l = new LinkedList<String>();
            for (int element : list) { 
                l.add(mapper(element));
            }
            return l;
        }
    }
```
Die verallgemeinerte Variante:
```java
    public interface List<T> {
        <R> List<R> map(Function<? super T, ? extends R> mapper) {
            List<R> l = new LinkedList<R>();
            for (T element : list) { 
                l.add(mapper(element));
            }
            return l;
        }
    }    
```
Wir sehen, dass hier die Implementation nicht schwieriger ist. Jedoch ist die verallgemeinerte Variante auf viel mehr Probleme anwendbar. 
Wir sehen jedoch auch, dass die Verallgemeinerung nicht ganz gratis kommt. Die Typsignaturen sind hier deutlich schwieriger zu lesen. 
Häufig dauert auch die Entwicklung einer allgemeinen Version deutlich länger. Es ist also immer gut abzuwägen, wie allgemein man werden will. 


## Schrittweises Entwickeln

Anstatt dass wir ein Problem auf einmal lösen, können wir versuchen die Lösung inkrementell zu entwickeln. 
Die erste Lösung ist dabei vielleicht nur eine grobe Approximation der endgültigen Lösung. In weiteren Schritten wird diese Approximation dann 
verbessert, bis wir den gewünschten Endzustand erreichen. 

Bei der Softwareentwicklung kann man zum Beispiel versuchen, zuerst eine minimale Menge von Funktionalitäten zu identifizieren, die für den Benutzer bereits
von Nutzen ist. Nachdem diese entwickelt sind, kann der Benutzer erstes Feedback geben, welches wieder in die nächsten Entwicklungsschritte einfliesst. 

Diese Inkrementelle Entwicklung ist die Grundlage von allen modernen Softwareprozessen, insbesondere auch der *Agilen Softwareentwicklung*. 

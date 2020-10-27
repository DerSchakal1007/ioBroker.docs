Modbus und Fröling Lambdatronic P/T/S 3200
==========================================

In diesem Tutorial wird die Einrichtung eines ModBus Adapters / Instance für die
Fröling Lambdatronic 3200 erklärt.

 

Voraussetzungen
---------------

-   iobroker master (Bereits bestehende iobroker Installation auf NAS / Raspi /
    etc.)

-   iobroker slave (Raspberry mit RS-232 USB Adapter) fertig installierte und
    konfigurierte Minimalinstallation von iobroker wie in der Dokumentation für
    [Multihost
    Modus](https://www.iobroker.net/docu/index-24.htm?page_id=3068&lang=de)
    beschrieben

 

iobroker slave
--------------

Unter

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$/dev 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

auf dem Raspi solltet Ihr ein Gerät wie ttyUSB0 finden, wenn das der Fall ist
könnt Ihr zum nächsten Punkt weiter gehen

 

Lambdatronic 3200 
------------------

Press Control+K to insert a hyperlink. It can be either literal URL
(<http://www.google.com/>) or have some [text](http://www.texts.io/). Click URL
with Control key pressed to open it in web browser.

 

iobroker master konfiguration
-----------------------------

Formulas can be placed inline like $$E=mc^2$$ or in a separate paragraph, like
the following one. Standard LaTeX syntax is supported.

$$
\frac{n!}{k!(n-k)!} = \binom{n}{k}
$$

Code
----

Inline `code` gets monospaced font.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Verbatim blocks use monospaced font as well and preserve line
breaks
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Press “Enter” key inside code block to insert a line break or “Shift+Enter” to
end code block.

Lists
-----

-   First bulleted item.

-   Second bulleted item.

Lists can be styled via menu, keyboard shortcut, or using autoformatting: type
minus and space for bulleted item or “1”, point and space for numbered item.
Press “Tab” to indent paragraph and create subitem, “Shift+Tab” to unindent.

1.  First numbered item.

2.  Second numbered item.

Tables
------

Press **Ctrl+Shift+L** to create a table, Ctrl+Alt+arrow keys to add more
columns or rows, Alt+Shift+arrow keys to move column or row. Here is a sample
table.

| **Features** | **Editable in Texts** | **Export to PDF** | **Export to HTML** |
|--------------|-----------------------|-------------------|--------------------|
| Basic Styles | Yes                   | Yes               | Yes                |
| Footnotes    | Yes                   | Yes               | Yes                |
| Images       | Yes                   | Yes               | Yes                |
| Tables       | Yes                   | Yes               | Yes                |

Happy writing!
==============

Got a question? Visit <http://www.texts.io/support/>.

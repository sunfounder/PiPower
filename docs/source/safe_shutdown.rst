.. note::

    Hallo und willkommen in der SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasten-Gemeinschaft auf Facebook! Tauchen Sie tiefer ein in die Welt von Raspberry Pi, Arduino und ESP32 mit anderen Enthusiasten.

    **Warum beitreten?**

    - **Expertenunterstützung**: Lösen Sie Nachverkaufsprobleme und technische Herausforderungen mit Hilfe unserer Gemeinschaft und unseres Teams.
    - **Lernen & Teilen**: Tauschen Sie Tipps und Anleitungen aus, um Ihre Fähigkeiten zu verbessern.
    - **Exklusive Vorschauen**: Erhalten Sie frühzeitigen Zugang zu neuen Produktankündigungen und exklusiven Einblicken.
    - **Spezialrabatte**: Genießen Sie exklusive Rabatte auf unsere neuesten Produkte.
    - **Festliche Aktionen und Gewinnspiele**: Nehmen Sie an Gewinnspielen und Feiertagsaktionen teil.

    👉 Sind Sie bereit, mit uns zu erkunden und zu erschaffen? Klicken Sie auf [|link_sf_facebook|] und treten Sie heute bei!

Sicheres Herunterfahren des Raspberry Pi mit PiPower
=======================================================

Das PiPower-Board bietet mehrere Erweiterungspins, die genutzt werden können, um Funktionen wie ein sicheres Herunterfahren des Raspberry Pi zu ermöglichen. Hier eine detaillierte Übersicht dieser Pins und ihrer Funktionen.

.. image:: img/io_pin.png
   :width: 500
   :align: center

* **GND**: Erdanschluss.
* **BT_LV**: Zeigt die Batteriespannung an, die 1/3 der tatsächlichen Batteriespannung entspricht.
* **IN_DT**: Dient zur Erkennung einer angeschlossenen USB-Stromversorgung. Gibt ein Hochsignal aus, wenn USB-Strom erkannt wird.
* **CHG**: Signalisiert, dass das Gerät geladen wird.
* **LO_DT**: Kennzeichnet einen niedrigen Batteriespannungsstatus. Gibt ein Hochsignal aus, wenn eine niedrige Batterie erkannt wird.
* **EN**: Dient als Schaltersignal. Bei Verbindung mit einem externen Schalter und Erdung wird das PiPower ausgeschaltet. Dies ist nur wirksam, wenn der integrierte Schalter aktiv ist.
* **LED**: Gibt eine Stromversorgungsanzeige aus. Liefert 5V bei eingeschaltetem Gerät. Bei Anschluss einer externen LED ist ein strombegrenzender Widerstand erforderlich.

.. note:: Diese Pins sind nicht vorverlötet und müssen mit einem Lötkolben verlötet werden.

In diesem Projekt konzentrieren wir uns auf die Pins **IN_DT**, **CHG** und **LO_DT**, um festzustellen, ob eine externe Batterie vorhanden ist, ob das USB-Ladekabel eingesteckt ist und ob die Batteriespannung niedrig ist. Dies stellt sicher, dass der Raspberry Pi bei niedrigem Batteriestand sicher herunterfährt.

.. warning:: Stecken Sie niemals gleichzeitig die externe Batterie und die mitgelieferte Batterie ein!

**Verkabelung**

Diese Tabelle zeigt, wie das PiPower mit dem Raspberry Pi verbunden werden sollte:

.. list-table:: 
    :widths: 50 50
    :header-rows: 1

    * - PiPower
      - Raspberry Pi
    * - IN_DT
      - GPIO17
    * - CHG
      - GPIO18
    * - LO_DT_PIN
      - GPIO27
    * - GND
      - GND

**Download und Test**

Ein Beispielcode für das sichere Herunterfahren wird bereitgestellt:

1. Herunterladen von `PiPower Github <https://github.com/sunfounder/pipower.git>`_ oder Klonen mittels:

    .. code-block::

        git clone https://github.com/sunfounder/pipower.git

2. Navigieren Sie zum Verzeichnis „examples“:

    .. code-block::

        cd pipower/examples

3. Führen Sie das Testprogramm aus, um die korrekte Erkennung der Stromzustände durch den Raspberry Pi zu überprüfen:

    .. code-block::

        python3 read_all.py

Sie können verschiedene Stromzustände simulieren, indem Sie das USB-Kabel entfernen, die Batterie herausnehmen oder die Pin-Verbindungen des Raspberry Pi ändern. Die ausgegebenen Meldungen zeigen den Stromzustand an. Zum Beispiel wird folgende Meldung angezeigt, wenn die Stromversorgung nur durch die Batterie erfolgt:

    .. code-block::

        External power disconnected
        Not charging
        Battery OK

.. warning:: Verbinden Sie niemals gleichzeitig die externe Batterie und die integrierte Batterie!

**Einrichten des sicheren Herunterfahrens**

Um die Funktion für das sichere Herunterfahren zu aktivieren:

1. Im Verzeichnis ``pipower/examples`` , führen Sie aus:

    .. code-block::

        sudo bash enable_safe_shutdown.sh

2. Starten Sie den Raspberry Pi neu:

    .. code-block::

        sudo reboot

Mit dieser Konfiguration wird Ihr Raspberry Pi automatisch heruntergefahren, wenn er nicht geladen wird oder die Batterie schwach ist.

**Erweiterte Konfigurationen**

Für individuelle Anpassungen können Sie weitere Aktionen in ``safe_shutdown.py`` hinzufügen. Fügen Sie unter ``# Do some stuff before shutting down`` den benötigten Code ein, um spezifische Aktionen vor dem Herunterfahren auszuführen, wie z.B. das Senden einer Benachrichtigung an Ihr Smartphone oder das Beenden bestimmter Dienste.

Denken Sie daran, ``enable_safe_shutdown.sh`` erneut auszuführen, wenn Sie Änderungen an ``safe_shutdown.py`` vornehmen.

    .. code-block::

        sudo bash enable_safe_shutdown.sh

# [ASKOHEAT+](https://www.askoma.com/de/produkte/produkte-fuer-den-pv-markt/askoheat.html) integration for [Home Assistant](https://www.home-assistant.io)

This is a set of configuration files to control some aspects of the [ASKOHEAT+](https://www.askoma.com/de/produkte/produkte-fuer-den-pv-markt/askoheat.html) heating element. I created these files for my own use. Use at your own risk!


## Integration with Home Assistant

The files are supposed to dropped in to `rest`, `rest_command`, and `switch` subfolders to the main Home Assistant configuration folder. They then can be loaded by adding the following lines to the `configuration.yaml` file:

```yaml
rest: !include_dir_merge_list rest 
rest_command: !include_dir_merge_named rest_command
switch: !include_dir_merge_list switch
```

See the following pages for more details:
* [`configuration.yaml`](https://www.home-assistant.io/docs/configuration/)
* [Splitting up the configuration](https://www.home-assistant.io/docs/configuration/splitting_configuration/)


## Please note...

I've found the devices built-in web server to be somewhat capricious. If queried too frequently it seems to crash every now and then. It then wouldn't respond for a minute or two before coming back up again. This would result in "not available" values in HA. But other than that it seems to work just fine. Except probably for the commands: if they were executed while the server was non-responsive, the commands obviously would not be executed.

As the non-responsiveness would result in a lot of errors logged to HA, I set logging to `fatal` for the `rest` integration, once I'm happy with it. 

```yaml
logging:
    default: warning
    logs:
        homeassistant.components.rest: fatal
```

Just make sure you reset this setting during development, or you might miss important messages!


## Some interesting links

The ASKOHEAT+ provides a simple JSON API to read/write important parameters.
http://download.askoma.com/askofamily_plus/Bedienungsanleitung_ASKOHEAT_plus.pdf

| BROWSER BEFEHL | BEMERKUNG |
| ----- | ----- |
| HTTP://ASKOHEAT.LOCAL/RESET | Neustart der Firmware |
| HTTP://ASKOHEAT.LOCAL/CHECK%20UPDATE | Prüfung auf eine neue Firmware |
| HTTP://ASKOHEAT.LOCAL/MAKE%20UPDATE | Start Update, wenn neue Firmware vorhanden |
| HTTP://ASKOHEAT.LOCAL/FORCE%20UPDATE | Update erzwingen (auch wenn die aktuelle Version schon die Aktuelle ist) |
| HTTP://ASKOHEAT.LOCAL/FULLSTATUS.JSON | Anzeige aktueller Statusinformationen und Einstellungen |
| HTTP://ASKOHEAT.LOCAL/GETALL | Anzeige Inhalt aller Modbus-Register |
| HTTP://ASKOHEAT.LOCAL/GETALL.JSON | Anzeige Inhalt aller Modbus-Register im JSON-Format |
| HTTP://ASKOHEAT.LOCAL/ON | Emergency On via Terminal / Browser |
| HTTP://ASKOHEAT.LOCAL/OFF | Emergency Off via Terminal / Browser |
| HTTP://ASKOHEAT.LOCAL/CLEAR%20TEMP%20ERROR | Temperatur-Sensor Fehler quittieren |
| HTTP://ASKOHEAT.LOCAL/IDENTIFY | Gerät identifizieren (LEDs blinken weiß) |
| HTTP://ASKOHEAT.LOCAL/FACTORY%20SET | Werkseinstellungen aller Parameter setzen |
| HTTP://ASKOHEAT.LOCAL/DEFAULT%20ANALOG%20IN | Werkseinstellungen nur für Analog Input Einstellungen setzen |
# ICS.php

ICS.php is a convenient script which generates iCalendar (.ics) files on the fly in PHP.

## Basic usage

```php
include 'ICS.php'

$properties = array(
  'dtstart' => 'now',
  'dtend' => 'now + 30 minutes'
);

$ics = new ICS($properties);
$ics_file_contents = $ics->to_string();
```

### Available properties

* **description** - string description of the event.
* **dtend** - date/time stamp designating the end of the event. You can use either a `DateTime` object or a [PHP datetime format string](http://php.net/manual/en/datetime.formats.php) (e.g. "now + 1 hour").
* **dtstart** - date/time stamp designating the start of the event. You can use either a `DateTime` object or a [PHP datetime format string](http://php.net/manual/en/datetime.formats.php) (e.g. "now + 1 hour").
* **location** - string address or description of the location of the event.
* **summary** - string short summary of the event - usually used as the title.
* **url** - string url to attach to the the event. Make sure to add the protocol (`http://` or `https://`).

## Detailed examples

### Button that downloads an ICS file when clicked

This example contains a form on the front-end that submits to a PHP script that initiates a download of an ICS file. This example uses hidden form fields to set the properties dynamically.

**index.html**

```html
<form method="post" action="/download-ics.php">
  <input type="hidden" name="date_start" value="2017-1-16 9:00AM">
  <input type="hidden" name="date_end" value="2017-1-16 10:00AM">
  <input type="hidden" name="location" value="123 Fake St, New York, NY">
  <input type="hidden" name="description" value="This is my description">
  <input type="hidden" name="summary" value="This is my summary">
  <input type="hidden" name="url" value="http://example.com">
  <input type="submit" value="Add to Calendar">
</form>
```

**download-ics.php**

```php
<?php

include 'ICS.php';

header('Content-Type: text/calendar; charset=utf-8');
header('Content-Disposition: attachment; filename=invite.ics');

$ics = new ICS(array(
  'location' => $_POST['location'],
  'description' => $_POST['description'],
  'dtstart' => $_POST['date_start'],
  'dtend' => $_POST['date_end'],
  'summary' => $_POST['summary'],
  'url' => $_POST['url']
));

echo $ics->to_string();
```

---
title: Jewish Calendar
description: Instructions on how to integrate the Jewish Calendar integration within Home Assistant.
ha_category:
  - Calendar
ha_iot_class: Local Polling
ha_release: 0.79
ha_codeowners:
  - '@tsvi'
---

The Jewish Calendar (`jewish_calendar`) sensor platform displays a variety of information related to the Jewish Calendar as a variety of sensors.

## Configuration

To enable this sensor in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
jewish_calendar:
```

{% configuration %}
language:
  required: false
  default: english
  description: Whether to represent the sensors in Hebrew (א' תשרי תשע"ט) or English characters (1 Tishrei 5779). Valid options are 'english' and 'hebrew'.
  type: string
latitude:
  required: false
  description: Latitude for time calculations of the sensor.
  default: Home Assistant location
  type: integer
longitude:
  required: false
  description: Longitude for time calculations of the sensor.
  default: Home Assistant location
  type: integer
diaspora:
  required: false
  description: Consider the location as diaspora or not for calculation of the weekly portion and holidays.
  default: false
  type: string
candle_lighting_minutes_before_sunset:
  required: false
  description: Number of minutes before sunset to report as candle lighting time.
  default: 18
  type: integer
havdalah_minutes_after_sunset:
  required: false
  description: Number of minutes after sunset to report as havdalah time. If this is set to 0, uses the time that the sun is 8.5 degrees below the horizon (same as the 'three_stars' sensor). If non-zero, this value is added as an offset to the time of sunset to report havdalah.
  default: 0
  type: integer
{% endconfiguration %}

## Sensor list

### Data sensors
- date: Shows the hebrew date for today.
- weekly_portion: Shows the weekly portion (parshat hashavu'a).
- holiday: If it is a holiday, shows the name of the holiday _(see below for more info)_.
- omer_count: An integer sensor indicating the day of the Omer (1-49) or 0 if it is not currently the Omer.

### Time sensors

*Note: Due to the variety of rabbinic opinions on how to calculate the different times, we do not take any responsibility on your religious reliance upon these calculations.*

Time sensor states are represented as ISO8601 formatted *UTC time*.
For easier use in automations, all time sensors have a `timestamp` attribute, which returns the UNIX timestamp.

- first_light: First light of dawn (Alot Hashachar - עלות השחר).
- talit: Earliest time for tallit and tefillin
- gra_end_shma: Last time for reading of the Shma according to the Gr"a.
- mga_end_shma: Last time for reading of the Shma according to the MG"A.
- gra_end_tefilla: Last time for full shacharit according to the Gr"a.
- mga_end_tefilla: Last time for full shacharit according to the MG"A.
- big_mincha: Earliest time for Mincha (Mincha Gedola)
- little_mincha: Preferable earliest time for Mincha (Mincha Ketana)
- plag_mincha: Time of the Plag Hamincha.
- sunset: Sunset (Shkiya)
- first_stars: Time at which the first stars are visible (Tseit Hakochavim - צאת הכוכבים).
- upcoming_shabbat_candle_lighting: The time of candle lighting for either the current Shabbat (if it is currently Shabbat) or the immediately upcoming Shabbat.
- upcoming_shabbat_havdalah: The time of havdalah for either the current Shabbat (if it is currently Shabbat) or the immediately upcoming Shabbat. If it is currently a three-day holiday, this value *could* be None (i.e. if holiday is Sat./Sun./Mon. and it's Saturday, there will be no shabbat_havdalah value. See comments in hdate library for details.)
- upcoming_candle_lighting: The time of candle lighting for either the current Shabbat OR Yom Tov, or the immediately upcoming Shabbat OR Yom Tov. If, for example, today is Sunday, and Rosh Hashana is Monday night through Wednesday night, this reports the candle lighting for Rosh Hashana on Monday night. This avoids a situation of triggering pre-candle-lighting automations while it is currently Yom Tov. To always get the Shabbat times, use the upcoming_shabbat_candle_lighting sensor.
- upcoming_havdalah: The time of havdalah for either the current Shabbat OR Yom Tov, or the immediately upcoming Shabbat OR Yom Tov. If, for example, today is Sunday, and Rosh Hashana is Monday night through Wednesday night, this reports the havdalah for Rosh Hashana on Wednesday night. To always get the Shabbat times, use the upcoming_shabbat_havdalah sensor.

### Binary sensors
- issur_melacha_in_effect: A boolean sensor indicating if melacha is currently not permitted. The value is true when it is currently Shabbat or Yom Tov and false otherwise.


### Holiday sensor

The holiday sensor includes 3 attributes: *type*, *type_id* and *id*.
The *type_id* is useful for cases to condition automations based on a range of types.

The following is the list of holidays the sensor knows about with their selected type:


| ID                   | English                    | Hebrew                | Type                          |
|----------------------|----------------------------|-----------------------|--------------------------------|
| erev_rosh_hashana    | Erev Rosh Hashana          | ערב ראש השנה          | EREV_YOM_TOV (2)              |
| rosh_hashana_i       | Rosh Hashana I             | א' ראש השנה           | YOM_TOV (1)                   |
| rosh_hashana_ii      | Rosh Hashana II            | ב' ראש השנה           | YOM_TOV (1)                   |
| tzom_gedaliah        | Tzom Gedaliah              | צום גדליה             | FAST_DAY (5)                  |
| erev_yom_kippur      | Erev Yom Kippur            | עיוה"כ                | EREV_YOM_TOV (2)              |
| yom_kippur           | Yom Kippur                 | יום הכפורים           | YOM_TOV (1)                   |
| erev_sukkot          | Erev Sukkot                | ערב סוכות             | EREV_YOM_TOV (2)              |
| sukkot               | Sukkot                     | סוכות                 | YOM_TOV (1)                   |
| hol_hamoed_sukkot    | Hol hamoed Sukkot          | חול המועד סוכות       | HOL_HAMOED (3)                |
| hoshana_raba         | Hoshana Raba               | הושענא רבה            | EREV_YOM_TOV (2)              |
| simchat_torah        | Simchat Torah              | שמחת תורה             | YOM_TOV (1)                   |
| chanukah             | Chanukah                   | חנוכה                 | MELACHA_PERMITTED_HOLIDAY (4) |
| asara_btevet         | Asara B'Tevet              | צום עשרה בטבת         | FAST_DAY (5)                  |
| tu_bshvat            | Tu B'Shvat                 | ט"ו בשבט              | MINOR_HOLIDAY (7)             |
| taanit_esther        | Ta'anit Esther             | תענית אסתר            | FAST_DAY (5)                  |
| purim                | Purim                      | פורים                 | MELACHA_PERMITTED_HOLIDAY (4) |
| shushan_purim        | Shushan Purim              | שושן פורים            | MELACHA_PERMITTED_HOLIDAY (4) |
| erev_pesach          | Erev Pesach                | ערב פסח               | EREV_YOM_TOV (2)              |
| pesach               | Pesach                     | פסח                   | YOM_TOV (1)                   |
| hol_hamoed_pesach    | Hol hamoed Pesach          | חול המועד פסח         | HOL_HAMOED (3)                |
| pesach_vii           | Pesach VII                 | שביעי פסח             | YOM_TOV (1)                   |
| yom_haatzmaut        | Yom HaAtzma'ut             | יום העצמאות           | MODERN_HOLIDAY (6)            |
| lag_bomer            | Lag B'Omer                 | ל"ג בעומר             | MINOR_HOLIDAY (7)             |
| erev_shavuot         | Erev Shavuot               | ערב שבועות            | EREV_YOM_TOV (2)              |
| shavuot              | Shavuot                    | שבועות                | YOM_TOV (1)                   |
| tzom_tammuz          | Tzom Tammuz                | צום שבעה עשר בתמוז    | FAST_DAY (5)                  |
| tisha_bav            | Tish'a B'Av                | תשעה באב              | FAST_DAY (5)                  |
| tu_bav               | Tu B'Av                    | ט"ו באב               | MINOR_HOLIDAY (7)             |
| yom_hashoah          | Yom HaShoah                | יום השואה             | MEMORIAL_DAY (8)              |
| yom_hazikaron        | Yom HaZikaron              | יום הזכרון            | MEMORIAL_DAY (8)              |
| yom_yerushalayim     | Yom Yerushalayim           | יום ירושלים           | MODERN_HOLIDAY (6)            |
| shmini_atzeret       | Shmini Atzeret             | שמיני עצרת            | YOM_TOV (1)                   |
| pesach_viii          | Pesach VIII                | אחרון של פסח          | YOM_TOV (1)                   |
| shavuot_ii           | Shavuot II                 | שני של שבועות         | YOM_TOV (1)                   |
| sukkot_ii            | Sukkot II                  | שני של סוכות          | YOM_TOV (1)                   |
| pesach_ii            | Pesach II                  | שני של פסח            | YOM_TOV (1)                   |
| family_day           | Family Day                 | יום המשפחה            | ISRAEL_NATIONAL_HOLIDAY (9)   |
| memorial_day_unknown | Memorial day for fallen whose place of burial is unknown | יום זכרון... | MEMORIAL_DAY (8) |
| rabin_memorial_day   | Yitzhak Rabin memorial day | יום הזכרון ליצחק רבין | MEMORIAL_DAY (8)              |
| zeev_zhabotinsky_day | Zeev Zhabotinsky day       | יום ז'בוטינסקי        | MEMORIAL_DAY (8)              |


## Full configuration sample

```yaml
# Example configuration.yaml entry
jewish_calendar:
  language: english
  diaspora: true
  havdalah_minutes_after_sunset: 50
```
